<h2 id="🧑🏻💻-목표">🧑🏻‍💻 목표</h2>
<ol>
<li>Tabs 컴포넌트와 Tab, TabPanel 같은 하위 컴포넌트를 각각의 파일에 분리.</li>
<li>네임스페이스 패턴(Tabs.Tab, Tabs.TabPanel)을 사용하여 코드 구조를 명확히 정리.</li>
<li>최종적으로 사용자는 다음처럼 간단히 사용할 수 있게 만들기</li>
</ol>
<pre><code>// 완성 예시

&lt;Tabs&gt;
  &lt;Tabs.Tab&gt;Tab 1&lt;/Tabs.Tab&gt;
  &lt;Tabs.Tab&gt;Tab 2&lt;/Tabs.Tab&gt;
  &lt;Tabs.Tab&gt;Tab 3&lt;/Tabs.Tab&gt;

  &lt;Tabs.TabPanel&gt;Content for Tab 1&lt;/Tabs.TabPanel&gt;
  &lt;Tabs.TabPanel&gt;Content for Tab 2&lt;/Tabs.TabPanel&gt;
  &lt;Tabs.TabPanel&gt;Content for Tab 3&lt;/Tabs.TabPanel&gt;
&lt;/Tabs&gt;</code></pre><h3 id="1-파일-구조">1. 파일 구조</h3>
<p>먼저 프로젝트 구조를 설계합니다. 컴포넌트와 Context 파일을 분리합니다.</p>
<pre><code>src/
  components/
    Tabs/
      Tabs.js         // 메인 Tabs 컴포넌트 (네임스페이스 정의)
      Tab.js          // Tab 컴포넌트
      TabPanel.js     // TabPanel 컴포넌트
      TabContext.js   // Tabs 상태 관리용 Context</code></pre><h3 id="2-tabs-상태-관리-tabcontextjs">2. Tabs 상태 관리: TabContext.js</h3>
<p>TabContext.js는 Tabs 상태를 관리하기 위해 Context를 생성합니다.</p>
<pre><code>// TabContext.js
import React, { createContext, useContext, useState } from &quot;react&quot;;

const TabContext = createContext();

export const TabProvider = ({ children }) =&gt; {
  const [activeIndex, setActiveIndex] = useState(0);

  return (
    &lt;TabContext.Provider value={{ activeIndex, setActiveIndex }}&gt;
      {children}
    &lt;/TabContext.Provider&gt;
  );
};

export const useTabContext = () =&gt; useContext(TabContext);</code></pre><h3 id="3-tab-컴포넌트-tabjs">3. Tab 컴포넌트: Tab.js</h3>
<p>Tab.js는 개별 탭 버튼을 정의합니다. 클릭하면 해당 탭을 활성화합니다.</p>
<pre><code>// Tab.js
import React from &quot;react&quot;;
import { useTabContext } from &quot;./TabContext&quot;;

const Tab = ({ children, index }) =&gt; {
  const { activeIndex, setActiveIndex } = useTabContext();

  return (
    &lt;button
      onClick={() =&gt; setActiveIndex(index)}
      style={{
        fontWeight: activeIndex === index ? &quot;bold&quot; : &quot;normal&quot;,
        padding: &quot;10px&quot;,
        cursor: &quot;pointer&quot;,
      }}
    &gt;
      {children}
    &lt;/button&gt;
  );
};

export default Tab;</code></pre><h3 id="4-tabpanel-컴포넌트-tabpaneljs">4. TabPanel 컴포넌트: TabPanel.js</h3>
<p>TabPanel.js는 각 탭의 콘텐츠를 렌더링합니다. 활성화된 탭에 해당하는 콘텐츠만 보입니다.</p>
<pre><code>// TabPanel.js
import React from &quot;react&quot;;
import { useTabContext } from &quot;./TabContext&quot;;

const TabPanel = ({ children, index }) =&gt; {
  const { activeIndex } = useTabContext();

  if (activeIndex !== index) return null;

  return &lt;div style={{ padding: &quot;10px&quot; }}&gt;{children}&lt;/div&gt;;
};

export default TabPanel;</code></pre><h3 id="5-tabs-메인-컴포넌트-tabsjs">5. Tabs 메인 컴포넌트: Tabs.js</h3>
<p>Tabs.js는 상위 컨테이너로, 네임스페이스를 정의하고 TabContext를 감싸 상태를 관리합니다.</p>
<pre><code>// Tabs.js
import React from &quot;react&quot;;
import { TabProvider } from &quot;./TabContext&quot;;
import Tab from &quot;./Tab&quot;;
import TabPanel from &quot;./TabPanel&quot;;

const Tabs = ({ children }) =&gt; {
  return &lt;TabProvider&gt;{children}&lt;/TabProvider&gt;;
};

// 네임스페이스 구성
Tabs.Tab = Tab;
Tabs.TabPanel = TabPanel;

export default Tabs;</code></pre><h3 id="6-app-컴포넌트-tabs-사용">6. App 컴포넌트: Tabs 사용</h3>
<p>App.js에서 Tabs와 관련 컴포넌트를 사용합니다.</p>
<pre><code>// App.js
import React from &quot;react&quot;;
import Tabs from &quot;./components/Tabs/Tabs&quot;;

const App = () =&gt; {
  return (
    &lt;Tabs&gt;
      &lt;Tabs.Tab index={0}&gt;Tab 1&lt;/Tabs.Tab&gt;
      &lt;Tabs.Tab index={1}&gt;Tab 2&lt;/Tabs.Tab&gt;
      &lt;Tabs.Tab index={2}&gt;Tab 3&lt;/Tabs.Tab&gt;

      &lt;Tabs.TabPanel index={0}&gt;Content for Tab 1&lt;/Tabs.TabPanel&gt;
      &lt;Tabs.TabPanel index={1}&gt;Content for Tab 2&lt;/Tabs.TabPanel&gt;
      &lt;Tabs.TabPanel index={2}&gt;Content for Tab 3&lt;/Tabs.TabPanel&gt;
    &lt;/Tabs&gt;
  );
};

export default App;</code></pre><hr />
<h2 id="동작-원리">동작 원리</h2>
<h3 id="tabs가-네임스페이스-역할">Tabs가 네임스페이스 역할:</h3>
<p>Tabs는 상위 컴포넌트로 TabContext를 제공하고, 하위 컴포넌트를 네임스페이스로 관리합니다.
Tabs.Tab, Tabs.TabPanel로 하위 컴포넌트에 접근 가능합니다.</p>
<h3 id="context로-상태-관리">Context로 상태 관리:</h3>
<p>TabContext에서 현재 활성화된 탭(activeIndex)의 상태를 저장.
Tab 컴포넌트는 onClick으로 setActiveIndex를 호출하여 상태를 변경.
TabPanel은 activeIndex에 따라 해당 콘텐츠만 렌더링.</p>
<h3 id="사용자는-네임스페이스로-접근">사용자는 네임스페이스로 접근:</h3>
<p>Tabs.Tab와 Tabs.TabPanel 처럼 사용하여 구조를 깔끔하게 유지.</p>
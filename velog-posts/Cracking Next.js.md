<h2 id="nextjs가-뭔데">Next.js가 뭔데?</h2>
<p>🫱🏻 리액트로 프로젝트를 시작할 때 권장되는 All in one 서버 사이드 랜더링 프레임워크이다.
여기서 말하는 서버 사이드 랜더링이란, 화면에 표시될 UI를 클라이언트가 아닌 서버에서 
그리는 행위를 말한다.</p>
<p><strong>별칭 : 앞으로 줄여서 말하겠음 너무 김</strong></p>
<ul>
<li>CSR : 클라이언트 사이드 랜더링</li>
<li>SSR : 서버 사이드 랜더링</li>
</ul>
<hr />
<h3 id="csr이랑-ssr이랑-무슨-차이인데">CSR이랑 SSR이랑 무슨 차이인데?</h3>
<p>우선 글로 정리하면 이렇다.</p>
<p>CSR은 초기 랜더링 될 때 빈 HTML과 JS를 로딩해서 하나의 페이지를 빠르게 로딩하여 보여줌 
SSR은 초기 랜더링 때 전체 HTML을 로딩해서 Hyduration 작업을 통해 React 버츄얼 돔으로 바꿈</p>
<blockquote>
<p>** CSR 코드 **</p>
</blockquote>
<pre><code>&lt;script src=&quot;https://unpkg.com/react@18/umd/react.development.js&quot;&gt;&lt;/script&gt;
&lt;script src=&quot;https://unpkg.com/react-dom@18/umd/react-dom.development.js&quot;&gt;&lt;/script&gt;
&lt;script src=&quot;https://unpkg.com/@babel/standalone/babel.min.js&quot;&gt;&lt;/script&gt;

&lt;div id=&quot;app&quot;&gt;
  &lt;!-- 리액트에서 DOM을 그려줄 영역 --&gt;
&lt;/div&gt;

&lt;script type=&quot;text/babel&quot;&gt;
// 여기서 babel이란 JSX를 일반 JavaScript로 변환하는 것을 말한다.
  function MyApp() {
    return (
      &lt;section&gt;
        &lt;h1&gt;Hello React&lt;/h1&gt;
        &lt;p&gt;리액트가 그려주는 UI&lt;/p&gt;
      &lt;/section&gt;
    );
  }

  const container = document.querySelector('#app');
  ReactDOM.createRoot(container).render(&lt;MyApp /&gt;);
&lt;/script&gt;</code></pre><p>리액트를 한번이라도 접해본 사람이라면 알 수 있을 것이다, 리액트 돔은 container라는 변수를
Root로 만들어 MyApp 컴포넌트를 랜더링 시키는 것으로 브라우저에 html을 노출한다.</p>
<blockquote>
<p>*<em>SSR 코드 *</em></p>
</blockquote>
<pre><code>&lt;script src=&quot;https://unpkg.com/react@18/umd/react.development.js&quot;&gt;&lt;/script&gt;
&lt;script src=&quot;https://unpkg.com/react-dom@18/umd/react-dom.development.js&quot;&gt;&lt;/script&gt;
&lt;script src=&quot;https://unpkg.com/@babel/standalone/babel.min.js&quot;&gt;&lt;/script&gt;

/* 서버에서 보내준 html 정보 */
&lt;div id=&quot;app&quot;&gt;
  &lt;section&gt;
      &lt;h1&gt;Hello React&lt;/h1&gt;
    &lt;p&gt;리액트가 그려주는 UI&lt;/p&gt;
  &lt;/section&gt;
&lt;/div&gt;
/* 서버에서 보내준 html 정보 */

/* 서버에서 보내준 html을 기반으로 만들어질 MyApp 컴포넌트 */
&lt;script type=&quot;text/babel&quot;&gt;
  function MyApp() {
    return (
      &lt;section&gt;
        &lt;h1&gt;Hello React&lt;/h1&gt;
        &lt;p&gt;리액트가 그려주는 UI&lt;/p&gt;
      &lt;/section&gt;
    );
  }

  const container = document.querySelector('#app');
  ReactDOM.hydrateRoot(container, &lt;MyApp/&gt;);
  /* 서버에서 보내준 html을 기반으로 만들어질 MyApp 컴포넌트 */
&lt;/script&gt;</code></pre><p>우선 &quot;app&quot; 이라는 Element에 HTML 코드가 그려진 채로 브라우저에서 받은 모습이 보인다.</p>
<p>이는 브라우저에서 자바스크립트를 실행하기도 전에 UI가 표시되고 화면에 표시할 데이터를 어느 정도 서버에서 갖춰준 상태로 브라우저에 보내주는 모습을 보여주고 있다.</p>
<h3 id="hydration">hydration</h3>
<p>서버사이드 랜더링에서는 서버에서 보내준 html을 기반으로 hydration(hydrate) 이라는 작업을 하게 되는데, 이 작업은 서버에서 미리 렌더링된 HTML을 React 컴포넌트로 변환하는 작업이다.</p>
<p><strong>전체적인 동작 과정</strong></p>
<ol>
<li><p>서버에서 미리 HTML을 렌더링해 클라이언트로 전송하게 되고 이 HTML은 사용자가 페이지를 
로드할 때 즉시 보여주게 된다. SEO(검색 엔진 최적화)에 유리하며, 초기 로딩 시간을 줄여줌</p>
</li>
<li><p>페이지가 로드된 후, React가 실행되며 hydrateRoot를 사용해 기존 HTML을 React 컴포넌트로 변환한다. 이렇게 하면 기존 HTML 구조를 유지하면서 React의 동적인 기능을 사용할 수 있다.</p>
</li>
</ol>
<hr />
<p>*<em>hydration의 조건 *</em></p>
<p>서버에서 위와 같은 HTML 파일을 주었을 때 앱 컴포넌트를 하이드레이션 하려면 앱 컴포넌트 내용이 #root 태그의 하위 요소의 내용이 같아야 한다.</p>
<pre><code>/* 서버에서 보내준 html 정보 */
&lt;div id=&quot;app&quot;&gt;
  &lt;section&gt;
      &lt;h1&gt;Hello React&lt;/h1&gt;
    &lt;p&gt;리액트가 그려주는 UI&lt;/p&gt;
  &lt;/section&gt;
&lt;/div&gt;
/* 서버에서 보내준 html 정보 */

중요 : 위와 아래의 html 구성이 같음

/* 서버에서 보내준 html을 기반으로 만들어질 MyApp 컴포넌트 */
&lt;script type=&quot;text/babel&quot;&gt;
  function MyApp() {
    return (
      &lt;section&gt;
          &lt;h1&gt;Hello React&lt;/h1&gt;
        &lt;p&gt;리액트가 그려주는 UI&lt;/p&gt;
      &lt;/section&gt;
    );
  }</code></pre><hr />
<h3 id="nextjs에서-html을-생성하는-방식">Next.js에서 HTML을 생성하는 방식</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jioo/post/648438ae-70d1-4651-8282-638e8c6f4d21/image.jpg" /></p>
<hr />
<h3 id="그래서-ssr이-대세가-된-이유">그래서 SSR이 대세가 된 이유</h3>
<p>서버 사이드 렌더링을 쓰는 목적은 크게 &quot;검색 엔진 최적화&quot;와 &quot;빠른 페이지 렌더링&quot;인데</p>
<p>*<em>1.SEO *</em></p>
<p>검색 엔진 최적화란 구글, 네이버와 같은 검색 사이트에서 검색했을 때 결과가 사용자에게 많이 노출될 수 있도록 최적화 마케팅적 기법이다. 특히, SNS에서 링크를 공유했을 때 해당 웹 사이트의 정보를 이미지와 설명으로 표시해주는 OG(Open Graph) Tag를 페이지 별로 적용하기 위해서는 더 서버 사이드 렌더링을 사용 해야된다.</p>
<p><strong>2. 빠른 페이지 로딩</strong></p>
<p>또한, 서버 사이드 렌더링은 빈 HTML 페이지를 받아 브라우저에서 그리는 클라이언트 사이드 렌더링과 다르게 서버에서 미리 그려서 브라우저로 보내주기 때문에 사용자가 홈페이지에 처음 접근했을 때 랜더링이 끝나는 시간을 단축 할 수 있다.</p>
<p><strong>3. 서버에서 실행되는 코드와 브라우저에서 실행되는 코드 분리</strong>
서버사이드 랜더링에서는 서버에 관련된 코드들을 따로 클라이언트에 관련된 코드들을 따로 분리 할 수 있다. 이 점은 보는 시각에 따라 장점이 될 수 있고 단점이 될 수 있다고 보는데 아직까지 나는 장점으로 보인다.</p>
<hr />
<h2 id="ssr-외에-nextjs를-쓰는-이유">SSR 외에 Next.js를 쓰는 이유</h2>
<h3 id="페이지-라우터">페이지 라우터</h3>
<p>프로젝트 루트 레벨에 pages 폴더를 생성하고 리액트 컴포넌트 파일을 추가하면 자동으로 
파일 기반의 라우팅이 구성된다. 이 페이지 라우팅이 없으면 일일히 직접 router.js에 PATH/NAME/Import되는 Component를 명시해줘야 되는 불편함이 있다.</p>
<p><strong>파일구조</strong></p>
<pre><code>components/
pages/
  login.jsx
package.json</code></pre><p>pages 폴더 밑에 <strong>Login</strong> 라는 페이지의 컴포넌트가 있다고 가정</p>
<pre><code>pages/login.jsx
export default function Login() {
  return &lt;div&gt;로그인&lt;/div&gt;
}</code></pre><p>이렇게 되면 웹 애플리케이션을 실행했을 때 사용자가 /login 이라는 URL로 접근이 가능해지고</p>
<p>Login 컴포넌트의 내용이 화면에 그려진다. 이와 같이 페이지 라우터에서는 pages 폴더 하위에 생성한 리액트의 파일 이름을 따라 라우팅이 구성된다.</p>
<p><strong>또한 pages 폴더 밑에 파일 뿐만 아니라 중첩된 폴더와 파일을 생성할 수도 있다.</strong></p>
<pre><code>pages/
  login.jsx ⬅️ `/login` URL로 접근
  blog/
    index.jsx ⬅️ `/blog` URL로 접근
  dashboard/
    setting/
      menu.jsx ⬅️ '/dashboard/setting/menu' URL로 접근</code></pre><hr />
<h3 id="앱-라우팅-134v-이상">앱 라우팅 (13.4v 이상)</h3>
<p>아직 실무에선 페이지 라우팅이 지배적이긴 하지만 만약 실무에서 라우팅에 사용된 디렉터리들 
마다 안에 Page.js라는 파일이 필수로 있을 땐 거기는 앱 라우팅을 사용한다고 보면 된다.</p>
<p><strong>app 폴더 기본 규칙</strong></p>
<p>앱 폴더 안에서는 page.js와 route.js를 제외하고는 모두 <strong>일반 파일</strong>로 취급된다.
설령 다음과 같이 폴더로 구분해 두더라도 page.js 파일이 없다면 라우팅 정보가 구성되지 않음</p>
<pre><code>app/
  login/
  dashboard/</code></pre><p>/login과 /dashboard에 해당하는 라우팅 정보를 구성하고 싶다면 폴더 안에 꼭 page.js를 생성해야 함</p>
<pre><code>app/
  login/
    page.js &lt;-- /login 로 접근 가능
  dashboard/
    page.js &lt;-- /dashboard 로 접근 가능</code></pre><hr />
<p><strong>혹은 api 폴더에 route.js를 선언해서 접근할 수 있게 할수도 있다.</strong></p>
<pre><code>app/
  login/
    page.js &lt;-- /login 로 접근 가능
  api/
    route.js &lt;-- /api 로 접근 가능</code></pre><hr />
<blockquote>
<p><strong>앱 라우터 내장 기능</strong></p>
</blockquote>
<p><strong>앱 라우터 폴더에는 다음 특정 파일들을 생성할 수 있다.</strong></p>
<ul>
<li>layout : 공통 UI(레이아웃)를 배치하는 컴포넌트</li>
<li>page : 특정 URL의 페이지 컴포넌트. page라는 이름을 붙이면 해당 URL로 접근 가능</li>
<li>loading : 해당 영역에 대한 로딩 컴포넌트</li>
<li>not-found : 페이지를 찾을 수 없습니다(404)에 해당하는 에러 바운더리 컴포넌트</li>
<li>error : 에러가 발생했을 때 에러 UI를 표시하는 에러 바운더리 컴포넌트</li>
<li>global-error : 전역 에러 UI를 표시하는 에러 처리 컴포넌트</li>
<li>route : API 엔드포인트로 취급되는 파일</li>
<li>template: 레이아웃 컴포넌트가 다시 그려질 때 표시될 UI</li>
</ul>
<hr />
<blockquote>
<p>*<em>프라이빗 폴더(Private Folder) *</em></p>
</blockquote>
<p>app 폴더의 기본 규칙을 우회하는 방법이 있는데, 폴더 앞에 _ 접두사를 붙이는 프라이빗 폴더를 정의한다.</p>
<pre><code>app/
  login/
    page.js &lt;-- /login 로 접근 가능
  _dashboard/ &lt;-- 프라이빗 폴더로 정의되어 접근 불가
    page.js </code></pre><p>위와 같이 _ 접두사를 붙이는 경우 해당 폴더는 프라이빗 폴더로 간주되는데 프라이빗 폴더 밑의 모든 파일은 모두 라우팅에서 배제된다.(URL로 접근할 수 없음)</p>
<p><strong>프로젝트 폴더 구조 전략 참고</strong>
<a href="https://cracking-next.vercel.app/docs/app-router/project-structure">https://cracking-next.vercel.app/docs/app-router/project-structure</a></p>
<hr />
<h3 id="에러-페이지-제공">에러 페이지 제공</h3>
<p>넥스트에서는 웹에서 볼 수 있는 에러 페이지 유형을 쉽게 커스텀 할 수 있도록 옵션을 제공</p>
<p>404 에러 페이지는 다음과 같이 pages/404.js로 생성한다.</p>
<p>커스텀 에러 파일은 <strong>페이지 컴포넌트</strong>로 취급되기 때문에 데이터 호출 메서드인 getStaticProps를 지원한다.</p>
<blockquote>
<p><strong>넥스트 기본 에러 페이지</strong></p>
</blockquote>
<p>만약 별도의 에러 페이지를 만들고 싶지 않다면 넥스트의 기본 에러 페이지를 사용할 수 있다.</p>
<pre><code>import Error from 'next/error'

export default function Page({ error }) {
  if (error) {
    return &lt;Error statusCode={error.status} /&gt;
  }
  return &lt;div&gt;Hello Next&lt;/div&gt;
}

export async function getServerSideProps() {
  try {
    const res = await fetch('https://api.github.com/repos/vercel/next.js')
  } catch(error) {
    return {
      props: { error }
    }
  }
}</code></pre><hr />
<h2 id="서버-컴포넌트">서버 컴포넌트</h2>
<p>CSR과 SSR의 한계를 보완하기 위해 나온 것이 서버 컴포넌트이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jioo/post/7c39ad9a-82a9-4c80-ba23-7ef24c2ff1c8/image.jpg" /></p>
<hr />
<p><img alt="" src="https://velog.velcdn.com/images/jioo/post/8961e080-33c5-4987-9a9c-65aaa44c844f/image.jpg" /></p>
<p>Next.js를 랜더링 방식은 서버에서 필요한 데이터를 받고 완성된 html을 브라우저에 
내보내기 때문에 최상단 레벨에서 밖에 작동을 안한다.</p>
<hr />
<h3 id="react-query-와-getserversideprops의-차이">React query 와 GetServerSideProps의 차이</h3>
<blockquote>
<p><strong>1. SEO가 중요한 경우</strong></p>
</blockquote>
<p>getServerSideProps는 서버에서 데이터를 가져와 완성된 HTML을 브라우저에 전달하기 때문에
검색 엔진이 내용을 잘 읽을 수 있습니다.</p>
<p>React Query는 클라이언트에서 데이터를 가져오기 때문에, 검색 엔진이 데이터를 못 읽을 가능성이 있습니다. 이런 경우, SEO가 중요한 페이지는 getServerSideProps를 사용해야 합니다.</p>
<blockquote>
<p><strong>2. 초기 로딩 속도가 중요한 경우</strong></p>
</blockquote>
<p>getServerSideProps를 사용하면 초기 화면이 완성된 상태로 로드되기 때문에, 
사용자가 페이지를 열자마자 모든 데이터를 볼 수 있습니다.</p>
<p>React Query는 클라이언트에서 데이터를 가져오므로, 로딩 상태가 보이고
데이터가 렌더링되기까지 약간의 시간이 필요합니다.</p>
<blockquote>
<p><strong>3. 인증 및 권한 확인</strong></p>
</blockquote>
<p>로그인 여부나 사용자 권한을 확인해야 하는 페이지에서는 서버에서 데이터를 확인한 후에 페이지를 렌더링해야 합니다. 이 경우, getServerSideProps가 적합합니다.</p>
<hr />
<h3 id="서버컴포넌트에서-api-요청을-하면-유리한-경우">서버컴포넌트에서 API 요청을 하면 유리한 경우</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jioo/post/2f11e9c5-663d-47f6-8b90-bb45f6a3913f/image.jpg" /></p>
<hr />
<h2 id="그외">그외</h2>
<ul>
<li>데이터 호출시 네트워크 비용 감소</li>
<li>보안 강화</li>
<li>캐싱</li>
<li>저사양기기까지 고려한 성능 이점</li>
<li>초기 페이지 로딩 속도 향상</li>
<li>SEO와 SNS 프리뷰(OG 태그)</li>
</ul>
<p>넥스트에서는 위와 같은 장점이 있어서 기본 컴포넌트 타입이 서버 컴포넌트로 설정되어 있다.</p>
<p>그렇게 때문에 리액트의 use hooks, onClick 등 상태나 핸들러를 선언 할 수 없다. </p>
<p>만약 특정 컴포넌트에 use hooks, onClick 등 상태나 핸들러를 선언하고 싶다면 
꼭 컴포넌트 파일 최상단에 'use client'를 붙히지 않으면 에러가 발생하니 주의하자.</p>
<hr />
<p>여기서 주의해야 할 점은 모든 클라이언트 컴포넌트에 'use client'를 붙일 필요는 없고 
한번 'use client'를 붙였다면 해당 컴포넌트의 자식 컴포넌트들은 모두 클라이언트 컴포넌트로  간주되기 때문에, 클라이언트 컴포넌트는 최대한 컴포넌트 트리 아래 깊숙히 배치해야한다. </p>
<p>🙅🏻 <strong>가능한한 컴포넌트들을 모두 서버 컴포넌트로 지정하라는 말이다.</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/jioo/post/708aa5bf-7bb7-4508-9bc8-9cb6a68e89fa/image.jpg" /></p>
<p>캡틴판교님 이미지 인용</p>
<hr />
<h2 id="💡요약">💡요약</h2>
<p>Next.JS는 React와 다르게 SSR로 만들어져 있어서 처음부터 서버에서 가져온 데이터를 가지고
완성된 HTML를 가져옴,</p>
<p>그렇게 가져온 HTML의 dom을 React에서 쓸 수 있는 버츄얼 돔 으로 변환 하는 hydration 작업을 함</p>
<p>이는 검색엔진이 완성된 HTML을 서치하는 것 이기 때문에 SEO에 유리하며 데이터를 가져오는 것을 랜더링 보다 먼저 하기 때문에 초기 페이지 로딩이 빠르다.</p>
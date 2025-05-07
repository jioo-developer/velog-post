<p><strong>🔍 Vue에서 ref와 DOM 연결 시점: onMounted() 안에서만 써야 할까?</strong></p>
<p>Vue 3 Composition API를 사용하면서 ref로 DOM 요소를 잡고,
그걸 커스텀 훅(useXXX)이나 기능에 넘겨주는 경우, 이런 고민이 생긴다</p>
<p>🤔 &quot;ref로 DOM 요소를 연결해서 훅에 넘기고 싶은데,
onMounted() 안에서 써야 안전할까? 아니면 그냥 setup() 안에서 써도 될까?&quot;</p>
<blockquote>
<p>✅ <strong>안전한 구조 예시</strong></p>
</blockquote>
<pre><code>&lt;template&gt;
  &lt;div ref=&quot;platform2&quot;&gt;&lt;/div&gt;
&lt;/template&gt;

&lt;script setup&gt;
import { ref } from 'vue'
import useScrollTrigger from '@/hooks/useScrollTrigger'

const platform2 = ref(null)
const { isVisible: playFormState } = useScrollTrigger(platform2) 
// ✅ setup() 안에서 바로 사용해도 OK

&lt;/script&gt;
왜 이게 가능할까?
👉 핵심은 훅 내부에 onMounted() 가 있기 때문입니다.

export default function useScrollTrigger(targetRef) {
  const isVisible = ref(false)

  onMounted(() =&gt; {
    if (!targetRef.value) return

    const observer = new IntersectionObserver(([entry]) =&gt; {
      isVisible.value = entry.isIntersecting
    }, {
      threshold: 0.5
    })

    observer.observe(targetRef.value)
  })

  return { isVisible }
}</code></pre><blockquote>
<p>✅ <strong>ref가 DOM에 연결되는 순서 (쉽게 설명한 6단계)</strong></p>
</blockquote>
<ol>
<li><p>setup() 함수가 실행돼요
→ 이때 ref(null)을 해서 ref가 만들어지지만
👉 아직 화면에 DOM은 없어요
👉 그래서 ref.value === null</p>
</li>
<li><p>useScrollTrigger(ref)가 호출돼요
→ 이때 훅 내부에서는 onMounted()를 등록만 해요
👉 즉, &quot;나중에 화면 다 만들어지면 이거 실행해줘!&quot; 라고 예약만 하는 거예요</p>
</li>
<li><p>Vue가 template영역을 보고 실제 DOM을 만들어요
→ div ref=&quot;platform2&quot;가 브라우저 화면에 나타남</p>
</li>
<li><p>이제 onMounted()가 실행돼요
→ 이때 ref.value는 드디어 진짜 DOM 요소를 가리켜요
👉  엘리먼트에 연결 완료!</p>
</li>
<li><p>IntersectionObserver.observe(ref.value) 실행돼요
→ 이때는 ref.value가 실제 DOM이니까,
👉 observer도 정상 작동! ✅</p>
</li>
<li><p>화면 스크롤할 때 observer가 작동해요
→ isVisible.value = true처럼 값이 바뀌고, 애니메이션도 시작!</p>
</li>
</ol>
<p>❌ 이렇게 하면 안 돼요 (위험한 구조)</p>
<pre><code>const observer = new IntersectionObserver(...)
observer.observe(ref.value) // ❌ 이건 너무 빨라서 아직 null임!
→ ref.value는 아직 연결 전이라 에러 납니다!</code></pre><p>✅ <strong>기억할 것 하나!</strong></p>
<p>DOM 조작은 항상 onMounted() 이후에!
ref는 setup()에서 만들지만, 진짜 연결되는 시점은 onMounted()부터</p>
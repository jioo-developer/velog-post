<blockquote>
<p> <strong>JS doc이란?</strong>
타입스크립트를 쓸 수 없는 환경에서 자바스크립트로 타입의 힌트를 주는 라이브러리<br />
<strong>설치</strong></p>
</blockquote>
<pre><code>npm -i -D jsdoc

**jsconfig.json 파일 생성**</code></pre><p>{
 &quot;compilerOptions&quot;: {
  &quot;allowJs&quot; : true,
  &quot;checkJs&quot; : true
 }
}</p>
<p>//jsconfig.json 이 부분이 타입스크립트 처럼 에디터에서 타입이 안맞을 경우 주의/오류를 표시</p>
<pre><code>---
#### 기본 사용법</code></pre><p>/**</p>
<p>*/ </p>
<p>/**로 시작 */로 끝냄 </p>
<pre><code>
# 주요 태그 
## @param
함수의 매개변수를 설명할 때 사용 이 태그 뒤에 타입과 이름 설명을 작성

&gt; **예시코드**
</code></pre><p>/**</p>
<ul>
<li><p>이 함수는 사용자 정보를 매개변수로 받아, 사용자의 이름과 나이를 출력합니다.</p>
</li>
<li><p>@param {{name: string, age?: number}} person - name과 age 속성을 가진 사용자 객체</p>
<p>// person은 명확한 단어 작명 ?로 필수 / 선택을 구분 할 수 있다. 보통 작명되는 단어는
// 변수명이나 parameter argument로 쓰여질 단어로 작명한다</p>
</li>
<li><p>/</p>
</li>
</ul>
<p>function printUserInfo(person) {
    console.log(<code>Name: ${person.name}, Age: ${person.age ?? 'Unknown'}</code>);
}</p>
<p>// 함수를 호출하여 사용자 정보를 전달
printUserInfo({name: &quot;Alice&quot;, age: 30});  // 출력: Name: Alice, Age: 30
printUserInfo({name: &quot;Bob&quot;});     </p>
<pre><code>---

**숫자 예시 **</code></pre><p>/**</p>
<ul>
<li>이 함수는 두 숫자를 매개변수로 받아서 그 합을 반환합니다.</li>
<li>@param {number} a - 첫 번째 숫자</li>
<li>@param {number} b - 두 번째 숫자</li>
<li>@returns {number} - 두 숫자의 합</li>
<li>/
function add(a, b) {
  return a + b;
}</li>
</ul>
<p>// 함수를 호출하여 두 숫자의 합을 출력
console.log(add(3, 4));  // 출력: 7
console.log(add(10, 20));  // 출력: 30</p>
<pre><code>---

**문자열 예시 **</code></pre><p>/**</p>
<ul>
<li>이 함수는 문자열을 매개변수로 받아서 대문자로 변환하여 반환합니다.</li>
<li>@param {string} str - 변환할 문자열</li>
<li>@returns {string} - 대문자로 변환된 문자열</li>
<li>/
function toUpperCase(str) {
  return str.toUpperCase();
}</li>
</ul>
<p>// 함수를 호출하여 문자열을 대문자로 변환하여 출력
console.log(toUpperCase(&quot;hello&quot;));  // 출력: HELLO
console.log(toUpperCase(&quot;world&quot;));  // 출력: WORLD</p>
<pre><code>
---

## @type
변수에 타입을 표기할 때 사용

&gt; **예시코드**
</code></pre><p>/** @type {number} * /
let width; // 타입을 선언 한 후 밑에 변수를 선언</p>
<p>/** @type {boolean} * /
let isReady = false;</p>
<p>/** @type {string[]} * /
let fruits = ['Apple','Banana','Mango']</p>
<p>width = '100px'; //문자열이 같이 들어가서 에러
isReady = 'true';  // 성공
fruits = ['Apple','Banana','Mango',1] // string 배열에 숫자가 들어가서 에러</p>
<pre><code>---

## @typedef
typescript의 interface 역할이 필요할 때 사용

&gt; **예시코드**
</code></pre><p>/**
@typedef {Object} Person 
@typedef {string} name 
@typedef {string | number} id<br />@typedef {number} age 
*/</p>
<p>/**
type {person} * /
const person = {
 name : 'mike',
 id : 327,
 age : 39
}</p>
<p>person.email = <del>@ ~</del>.com // 에러 email 항목은 없음
*/</p>
<pre><code>
---

## @callback

&gt; **예시코드**

![](https://velog.velcdn.com/images/jioo/post/155e2265-36a3-4ade-88b4-d6d427811466/image.jpg)


## @this

&gt; **예시코드**
</code></pre><p>/**</p>
<ul>
<li>@this {HTMLElement}</li>
<li>@param {*} e</li>
<li>/</li>
</ul>
<p>function callbackForLater(e) {
  this.clientHeight = parseInt(e); // 잘 작동해야 합니다!
}</p>
<pre><code>---

# 참고사항
## @ts-nocheck
해당 파일의 모든타입 체킹 무시

&gt; **예시코드**
</code></pre><p>// @ts-nocheck</p>
<p>/** @type {number} */
let id = 100;
id = &quot;100&quot; // 에러 안남</p>
<pre><code>
## @param {*} data 
 @param {*} data는 typescript에서 any를 뜻한다

 &gt; **예시코드**
</code></pre><p>/<em>* @param {</em>} data */</p>
<p>function handleData(data) {
   console.log(data)
}</p>
<pre><code>



</code></pre>
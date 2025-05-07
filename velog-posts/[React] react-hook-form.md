<blockquote>
<p><strong>use Form 사용기</strong></p>
</blockquote>
<p>본 포스팅은 밑의 가이드를 보고 포스팅 함을 알립니다. (보다 모르겠으면 이거 보셈)
<a href="https://mycodings.fly.dev/blog/2023-09-10-all-in-one-about-react-hook-form">https://mycodings.fly.dev/blog/2023-09-10-all-in-one-about-react-hook-form</a></p>
<h3 id="기본연결">기본연결</h3>
<pre><code>const {register} = useform();
// 이렇게 불러와서 

&lt;form&gt;
  &lt;div className='w-1/3'&gt;
    &lt;label htmlFor='username'&gt;Username&lt;/label&gt;
    &lt;input type='text' id='username' {...register('username')} /&gt;
    &lt;label htmlFor='email'&gt;E-mail&lt;/label&gt;
    &lt;input type='email' id='email' {...register('email')} /&gt;
    &lt;label htmlFor='password'&gt;Password&lt;/label&gt;
    &lt;input type='password' id='password' {...register('password')} /&gt;
    &lt;button&gt;Submit&lt;/button&gt;
  &lt;/div&gt;
&lt;/form&gt;
// 연결 예시</code></pre><h3 id="react-hook-form-devtool">React hook form DevTool</h3>
<p>React-Hook-Form은 Devtool을 제공, Form 관리를 일일이 콘솔 창에 출력하지 않고 
인풋에 입력되는 값을 실시간으로 추적 가능</p>
<pre><code>const {register,control} = useform();
// 이렇게 불러 온 후

&lt;DevTool control={control}
// 이렇게 인풋 코드들의 맨밑에 불러오면 DevTool 사용 가능</code></pre><h3 id="onsubmit-하는법">onSubmit 하는법</h3>
<pre><code>
const submit = (arg)=&gt;{
 console.log(arg)
}

&lt;form onsubmit={handleSubmit(submit(params))}
// handleSubmit 고정함수에 submit 함수를 넣어 사용하는 것이 관습</code></pre><h3 id="유효성-검증">유효성 검증</h3>
<blockquote>
<p>Form의 유효성 검증으로 React-Hook-Form이 제공하는 기본 검증은 밑과 같다</p>
</blockquote>
<ul>
<li>required</li>
<li>minLength &amp; maxLength</li>
<li>min &amp; max</li>
<li>pattern // 정규식 유효성 검증<br /></li>
<li><em>예시*</em><pre><code>&lt;input
 type='email'
 id='email'
 {...register('email', {
   pattern: {
     value:
       /^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/,
     message: 'Email is invalid.',
   },
 })}
/&gt;</code></pre></li>
</ul>
<h3 id="에러-표시">에러 표시</h3>
<pre><code>const {formState: { errors }} = useForm&lt;FormValues&gt;();

&lt;p&gt;{errors.username?.message}&lt;/p&gt;
&lt;p&gt;{errors.email?.message}&lt;/p&gt;
&lt;p&gt;{errors.password?.message}&lt;/p&gt;

// 만약 userName에 유효성 검증이 두개 있을 경우

error.userName.type === &quot;검증하는 타입&quot; &amp;&amp; &lt;p&gt;&quot;에러 메시지 수동으로&quot;&lt;/p&gt;</code></pre><h3 id="커스텀-validation-함수-만들기">커스텀 Validation 함수 만들기</h3>
<pre><code>&lt;input type='email' id='email'
  {...register('email', {
    pattern: {
      value:
        /^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/,
      message: 'Email is invalid.',
    },
    validate: {
      noAdmin: fieldValue =&gt; {
         if (fieldValue === 'admin@fly.dev') {
          return 'You can not use admin@fly.dev!';
          }
      }
    },
  })}
/&gt;</code></pre><h3 id="default-values">default values</h3>
<pre><code>type FormValues = {
  username: string,
  email: number,
  password: Date,
}

const { register} = useForm &lt;FormValues&gt; {
  defaultValues: {
    username: 'IU',
    email: '',
    password: '',
  },
}</code></pre><h3 id="watch를-이용해서-실시간으로-formvalues-감시하기">watch를 이용해서 실시간으로 FormValues 감시하기</h3>
<pre><code>const {
    register,
    control,
    handleSubmit,
    watch,
    formState: { errors },
  } = useForm&lt;FormValues&gt;({
    defaultValues: {
      username: &quot;IU&quot;,
      age: 0,
      dob: new Date(),
    },
  });

  const watchForm = watch(&quot;username&quot;);
  const watchForm2 = watch(&quot;age&quot;);

  renderCount++;
  return (
    &lt;div className=&quot;w-full p-4&quot;&gt;
      &lt;h1 className=&quot;text-2xl mb-5&quot;&gt; Render count : {renderCount / 2}&lt;/h1&gt;
      &lt;h2 className=&quot;text-xl mb-5&quot;&gt;
        watch value : {watchForm} and {watchForm2}
      &lt;/h2&gt;
    ...
    ...
    ...
  )
}</code></pre><blockquote>
<p>*<em>무조건 watch는 useEffect에 넣어서 사용 *</em></p>
</blockquote>
<pre><code>import { useEffect } from 'react'

const {
  register,
  control,
  handleSubmit,
  watch,
  formState: { errors },
} = useForm &lt;
FormValues &gt;
{
  defaultValues: {
    username: 'IU',
    age: 0,
    dob: new Date(),
  },
}

useEffect(() =&gt; {
  const subscription = watch(value =&gt; {
    console.log(value)
  })

  return () =&gt; subscription.unsubscribe()
}, [watch])</code></pre><h3 id="폼-항목-disable-하기">폼 항목 disable 하기</h3>
<p>disabled를 watch함수와 같이 쓸 수 있는데요.</p>
<pre><code>&lt;input
  type=&quot;number&quot;
  id=&quot;age&quot;
  {...register(&quot;age&quot;, {
    disabled: watch(&quot;username&quot;) === &quot;&quot;,
    valueAsNumber: true,
    required: &quot;Age is required.&quot;,
  })}
/&gt;</code></pre><h3 id="handlesubmit-에러-발생시">handleSubmit 에러 발생시</h3>
<p>useForm() 함수가 리턴 하는 handleSubmit 함수를 이용해서 폼의 Submit 핸들링을 
담당하는 함수를 지정했었는데, Submit이 실패했을 때도 핸들링할 수 있는 함수를 제공함</p>
<p>handleSubmit 함수에 두 번째 인자가 폼 Submit이 실패했을 때 실행되는 콜백함수이다</p>
<pre><code>&lt;form onSubmit={handleSubmit(onSubmit, onError)} noValidate&gt;
위 코드와 같이 onSubmit 함수는 우리가 Submit이 성공했을 때 작동되도록 만든 함수이고,

onError 함수는 Submit이 실패했을 때 작동되도록 만들 예정인 함수입니다

import { useForm, useFieldArray, FieldErrors } from &quot;react-hook-form&quot;;

  const onError = (errors: FieldErrors&lt;FormValues&gt;) =&gt; {
    console.log(&quot;Form errors&quot;, errors)
  }</code></pre><h3 id="submission-상태값과-reset-함수">Submission 상태값과 reset 함수</h3>
<blockquote>
<p>React-Hook-Form의 formState에는 폼 Submission 상태 값이 4개가 있다.</p>
</blockquote>
<ul>
<li>isSubmitting,</li>
<li>isSubmitted,</li>
<li>isSubmitSuccessful,</li>
<li>submitCount<br />
isSubmitting 상태는 submit 버튼을 눌렀을 때고,
isSubmitted 상태는 submit이 눌러져서 벌써 액션이 취해졌을 때,
isSubmitSuccessful은 submit이 성공적으로 작동했을 때,
submitCount는 성공적인 submit의 갯수를 나타냄.<br /></li>
<li><em>응용 예시*</em><br /><pre><code>useEffect(()=&gt;{
 if(isSubmitSuccessful) { 
   reset()
 }
},[isSubmitSuccessful, reset])</code></pre></li>
</ul>
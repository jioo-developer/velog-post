<pre><code>const { userType } = userAccountInfo;
        if (userType === 'USER_TYPE_A') return &lt;UserATypeBanner /&gt;
        else if (userType === 'USER_TYPE_B') return &lt;UserBTypeBanner /&gt;
        else if (userType === 'USER_TYPE_C') return &lt;UserCTypeBanner /&gt;</code></pre><p> 예를 들어, USER_TYPE_D라는 새로운 유형이 추가되면, 기존 if/else 문에 
 새로운 else if 조건을 추가 해야 한다. </p>
<p>이 경우, 기존 코드에 직접 수정이 가해지므로 기존 코드의 변경이 발생하기 때문에 
원칙에 위배됨</p>
<pre><code>const BANNER_INFO = {
  USER_TYPE_A: {
    bannerType: 'bannerA',
    title: 'Welcome User A',
  },
  USER_TYPE_B: {
    bannerType: 'bannerB',
    title: 'Welcome User B',
  },
  USER_TYPE_C: {
    bannerType: 'bannerC',
    title: 'Welcome User C',
  }
}

const ShoppingPage = () =&gt; {
  // ...
  return (
    &lt;&gt;
      {userAccount.map((userAccountInfo) =&gt; { 
        const { userType } = userAccountInfo;
        return (
          &lt;UserBanner
            bannerType={BANNER_INFO[userType].bannerType}
            title={BANNER_INFO[userType].title}
          /&gt;
        );
      })}
    &lt;/&gt;
  );
}</code></pre><p>새로운 유저 유형(USER_TYPE_D)이 추가될 경우, BANNER_INFO 객체에 USER_TYPE_D에 
해당하는 배너 정보를 추가하면 된다.</p>
<p>기존 코드에는 아무런 변경 없이 새로운 유저 유형을 추가할 수 있음
즉, 기존 코드를 수정하지 않고도 기능을 확장할 수 있다.</p>
<p><strong>ISP 인터페이스 분리 원칙이란 &quot;객체는 자신이 사용하는 메소드에만 의존해야 한다&quot;는 원칙</strong></p>
<p>BAD 예시:</p>
<pre><code>const Notification = ({ project, user }: NotificationProps) =&gt; {
  if (project) {
    // Display a project notification
    return &lt;p&gt;{project?.name}&lt;/p&gt;;
  } else if (user) {
    // Display a user notification
    return &lt;p&gt;{user?.email}&lt;/p&gt;;
  } else {
    return null;
  }
};</code></pre><pre><code>
GOOD 예시:
const UserNotification = ({ user }: UserNotificationProps) =&gt; {
  return &lt;p&gt;{user?.email}&lt;/p&gt;;
};

const ProjectNotification = ({ project }: ProjectNotificationProps) =&gt; {
  return &lt;p&gt;{project?.name}&lt;/p&gt;;
};</code></pre><p>인터페이스 분리 원칙에서 말하는 &quot;객체는 자신이 사용하는 메소드에만 의존해야 한다&quot;는 말은,
컴포넌트가 자신이 사용하는 필요한 데이터만 받아야 한다는 뜻이다. 즉, 컴포넌트가 
여러 가지 다른 종류의 데이터를 하나의 props로 받지 않도록 하자</p>
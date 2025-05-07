<blockquote>
<p><strong>데이터 유형 제어 설명</strong></p>
</blockquote>
<p>부울은 가능한 상태들 간의 전환을 위한 토글을 제공한다.</p>
<ul>
<li><p>argType: {active: {control: 'boolean'}}
number number 가능한 모든 값의 범위를 포함하는 숫자 입력을 제공합니다.</p>
</li>
<li><p>argType: {even: {control: { type: 'number', min:1, max:30, step: 2 } }
range 가능한 모든 값을 포함하도록 범위 슬라이더 구성 요소를 제공합니다.</p>
</li>
<li><p>argType: { 홀수: {control: { type: 'range', min: 1, max: 30, step: 3 } }
object object 개체 값을 처리하기 위해 JSON 기반 편집기 구성 요소를 제공합니다.
또한 원시 모드로 편집할 수 있습니다.</p>
</li>
<li><p>argType: { 사용자: {control: 'object'}}
array object 배열의 값을 처리할 JSON 기반 편집기 구성 요소를 제공합니다.
또한 원시 모드로 편집할 수 있습니다.</p>
</li>
<li><p>argType: {홀수: {control: 'object'}}</p>
</li>
</ul>
<blockquote>
<p><strong>사용 예시 코드</strong></p>
</blockquote>
<pre><code>import type { Meta } from '@storybook/your-framework';

import { Gizmo } from './Gizmo';

const meta: Meta&lt;typeof Gizmo&gt; = {
  component: Gizmo,
  argTypes: {
    canRotate: {
      control: 'boolean',
    },
    width: {
      control: { type: 'number', min: 400, max: 1200, step: 50 },
    },
    height: {
      control: { type: 'range', min: 200, max: 1500, step: 50 },
    },
    rawData: {
      control: 'object',
    },
    coordinates: {
      control: 'object',
    },
    texture: {
      control: {
        type: 'file',
        accept: '.png',
      },
    },
    position: {
      control: 'radio',
      options: ['left', 'right', 'center'],
    },
    rotationAxis: {
      control: 'check',
      options: ['x', 'y', 'z'],
    },
    scaling: {
      control: 'select',
      options: [10, 50, 75, 100, 200],
    },
    label: {
      control: 'text',
    },
    meshColors: {
      control: {
        type: 'color',
        presetColors: ['#ff0000', '#00ff00', '#0000ff'],
      },
    },
    revisionDate: {
      control: 'date',
    },
  },
};

export default meta;</code></pre>
비유 싹 빼고, 수학적 증명 과정으로 바로 들어가겠습니다. 수식을 원하셨으니 최적화 이론의 본질인 <b>초평면 분리 정리(Separating Hyperplane Theorem)</b>를 이용해 강한 쌍대성이 성립하는 이유를 증명해 드리겠습니다.

---

### 1. Primal 문제와 Dual 문제의 수학적 정의

먼저 SVM의 최적화 문제를 일반적인 형태의 <b>Primal 문제</b>로 정의합니다.
$x = (w, b)$라고 할 때,
$$p^* = \min_x f(x) \quad \text{subject to} \quad g_i(x) \le 0 \quad (i = 1, \dots, m)$$
* 목적 함수: $f(x) = \frac{1}{2}\|w\|^2$
* 제약 조건: $g_i(x) = 1 - y_i(w^T x_i + b)$
* $p^*$는 이 Primal 문제의 최적해(최솟값)입니다.

여기에 라그랑주 승수 $\alpha_i \ge 0$를 도입한 라그랑주 함수는 다음과 같습니다.
$$\mathcal{L}(x, \alpha) = f(x) + \sum_{i=1}^m \alpha_i g_i(x)$$

<b>Dual 문제</b>는 다음과 같이 정의됩니다.
$$d^* = \max_{\alpha \ge 0} \min_x \mathcal{L}(x, \alpha)$$
* $d^*$는 Dual 문제의 최적해(최댓값)입니다.

### 2. 약한 쌍대성 (Weak Duality) 증명

모든 $x$와 $\alpha \ge 0$에 대해 다음 부등식은 항상 성립합니다.
$$\min_{x'} \mathcal{L}(x', \alpha) \le \mathcal{L}(x, \alpha) \le \max_{\alpha' \ge 0} \mathcal{L}(x, \alpha')$$

가장 왼쪽 식은 Dual 목적 함수 $q(\alpha)$이고, 가장 오른쪽 식은 원래의 Primal 문제입니다. 따라서 각 식에 대해 $\alpha$의 최댓값과 $x$의 최솟값을 취하면 다음이 무조건 성립합니다.
$$d^* \le p^*$$
이것이 <b>약한 쌍대성</b>입니다.

### 3. 강한 쌍대성 ($p^* = d^*$) 증명을 위한 볼록 집합 구성

이제 왜 $p^* = d^*$ 가 되는지 기하학적/해석학적으로 증명합니다.
값들의 영역을 나타내는 다음과 같은 집합 $\mathcal{A}$를 정의합니다.
$$\mathcal{A} = \{ (u, t) \in \mathbb{R}^m \times \mathbb{R} \mid \exists x, \text{ s.t. } g_i(x) \le u_i, f(x) \le t \}$$

<b>핵심:</b> $f(x)$는 이차형식으로 <b>볼록 함수(Convex Function)</b>이고, $g_i(x)$는 선형식(Affine)이므로 당연히 볼록 함수입니다. 따라서 이 집합 <b>$\mathcal{A}$는 볼록 집합(Convex Set)이 됩니다.</b>

우리가 찾는 Primal 최적해 $p^*$는 이 집합 $\mathcal{A}$의 경계에 있는 점 $(0, p^*)$를 의미합니다. (즉 $u=0$일 때 $t$의 최솟값)
따라서, 집합 $\mathcal{A}$ 안에는 $u \le 0$ 이면서 $t < p^*$ 인 점은 존재하지 않습니다.



### 4. 초평면 분리 정리 (Separating Hyperplane Theorem) 적용

집합 $\mathcal{A}$가 볼록 집합이고, $(0, p^*)$ 아래의 영역 $\mathcal{B} = \{ (0, t) \mid t < p^* \}$ 와 겹치지 않으므로, 수학의 <b>초평면 분리 정리</b>에 의해 두 영역을 나누는 초평면(Hyperplane)이 반드시 존재합니다.

이 초평면의 법선 벡터를 $(\lambda, \mu)$라고 하면, 집합 $\mathcal{A}$의 모든 점 $(u, t)$에 대해 다음 부등식이 성립합니다.
$$\sum_{i=1}^m \lambda_i u_i + \mu t \ge \mu p^*$$

이때 $\mathcal{A}$의 정의상 $u_i$와 $t$는 무한히 커질 수 있으므로, 부등식이 항상 성립하려면 법선 벡터의 요소는 $\lambda_i \ge 0$ 이고 $\mu \ge 0$ 이어야만 합니다.

### 5. Slater's Condition과 강한 쌍대성의 완성

여기서 <b>슬레이터 조건(Slater's Condition)</b>이 개입합니다. 슬레이터 조건이 만족된다는 것은 $\mu > 0$ 임을 보장해줍니다. (만약 $\mu = 0$ 이라면, 조건을 엄격하게 만족하는 해가 존재한다는 전제에 모순이 생깁니다.)

$\mu > 0$ 이므로 양변을 $\mu$로 나눌 수 있습니다. $\alpha_i = \lambda_i / \mu$ 라고 정의합시다.
$$\sum_{i=1}^m \alpha_i u_i + t \ge p^*$$

원래 $x$에 대해 $u_i = g_i(x)$, $t = f(x)$인 점도 집합 $\mathcal{A}$에 속하므로 대입해봅니다.
$$f(x) + \sum_{i=1}^m \alpha_i g_i(x) \ge p^*$$
이 식은 곧 라그랑주 함수입니다.
$$\mathcal{L}(x, \alpha) \ge p^*$$

이 식이 모든 $x$에 대해 성립하므로, $x$에 대한 최솟값을 취해도 부등식은 유지됩니다.
$$\min_x \mathcal{L}(x, \alpha) \ge p^*$$

좌변은 앞서 정의한 Dual 함수의 목적 함수 $q(\alpha)$입니다.
$$q(\alpha) \ge p^*$$

Dual 최적해 $d^*$는 $q(\alpha)$의 최댓값이므로 당연히 $d^* \ge q(\alpha)$ 입니다. 즉,
$$d^* \ge p^*$$

앞서 2번에서 약한 쌍대성으로 $d^* \le p^*$ 를 증명했고, 지금 $d^* \ge p^*$ 를 증명했으므로, 결과적으로 다음이 성립합니다.
<b>$$d^* = p^*$$ (강한 쌍대성 증명 완료)</b>

---

### 💡 요약: SVM에서의 적용

SVM은 목적 함수 $\frac{1}{2}\|w\|^2$ 가 명백한 볼록 함수이고, 제약 조건 $1 - y_i(w^T x_i + b) \le 0$ 가 1차식(선형식, Affine)입니다. 

수학적으로 제약 조건이 모두 Affine인 경우, <b>완화된 슬레이터 조건(Refined Slater's Condition)</b>이 적용되어, 제약 조건을 만족하는 해가 하나라도 존재하기만 하면 무조건 $\mu > 0$ 이 보장됩니다. 

결론적으로, 초평면 분리 정리에 의해 Primal 문제와 Dual 문제는 완벽하게 일치($d^* = p^*$)하게 됩니다. 이것이 `min max` 와 `max min` 을 마음대로 뒤집어도 수식적으로 전혀 오류가 없는 이유입니다. KKT 조건 도출로 바로 넘어갈까요?

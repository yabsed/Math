슬라이드에서 **ADVANCED**라고 표시된 부분들은 퍼셉트론 알고리즘이 왜 수학적으로 완벽하게 작동하는지 그 **증명 과정**을 다루고 있습니다. 고등학생 수준에서 이 수학적 흐름을 놓치지 않도록, 수식을 하나하나 풀어서 아주 자세하게 설명해 드릴게요.

---

## 1. 선형 분리 가능성 (Linear Separability)
어떤 데이터셋 $D_n$이 **선형 분리 가능하다**는 말은, 모든 데이터를 단 하나의 직선(또는 평면)으로 완벽하게 가를 수 있다는 뜻입니다.

*   **수학적 정의:** 모든 $i$번째 데이터에 대해 다음을 만족하는 가중치 $\theta$와 편향 $\theta_0$가 존재해야 합니다.
    $$y^{(i)}(\theta^T x^{(i)} + \theta_0) > 0$$
*   이 조건이 만족되면 **훈련 오차(Training Error) $\epsilon_n(h)$가 0**이 되는 완벽한 분류기를 만들 수 있습니다.

---

## 2. 마진(Margin)의 수학적 정의
마진은 데이터가 경계선으로부터 얼마나 떨어져 있는지를 나타내는 '안전 거리'입니다.

*   **점과 평면 사이의 거리:** 점 $x$에서 하이퍼플레인($\theta, \theta_0$)까지의 거리는 다음과 같습니다.
    $$\text{distance} = \frac{\theta^T x + \theta_0}{||\theta||}$$
*   **라벨이 있는 점의 마진:** 여기에 정답 $y^{(i)}$($+1$ 또는 $-1$)를 곱하면 방향성까지 고려된 거리가 됩니다.
    $$\text{margin} = y^{(i)} \cdot \frac{\theta^T x^{(i)} + \theta_0}{||\theta||}$$
*   **데이터셋의 마진:** 전체 데이터셋 중 **가장 작은 마진값**을 그 데이터셋의 마진이라고 부릅니다.
    $$\gamma = \min_i \left( y^{(i)} \cdot \frac{\theta^T x^{(i)} + \theta_0}{||\theta||} \right)$$
    > **Study Question:** 만약 어떤 점이 잘못 분류되었다면 마진의 부호는 무엇일까요? 정답은 **음수($-$)**입니다. 예측값과 실제 정답의 부호가 다르기 때문이죠.

---

## 3. 퍼셉트론 수렴 정리의 증명 (Proof)
이제 가장 어려운 부분인 **"퍼셉트론이 왜 $(R/\gamma)^2$번 안에 끝나는가?"**에 대한 증명입니다. (이해를 돕기 위해 원점을 지나는 경우를 가정합니다).

### 가정 (Conditions)
1.  **조건 a (마진 존재):** 정답지 $\theta^*$가 있고, 모든 데이터는 최소 $\gamma$ 이상의 마진을 가짐.
    $$y^{(i)} \frac{\theta^{*T} x^{(i)}}{||\theta^*||} \ge \gamma$$
2.  **조건 b (데이터 범위 제한):** 모든 데이터의 크기는 $R$보다 작음 ($||x^{(i)}|| \le R$).

### 증명 단계
우리는 현재 학습 중인 $\theta^{(k)}$가 정답 $\theta^*$와 얼마나 비슷한지 **코사인 유사도**($\cos \phi$)로 측정할 것입니다.
$$\cos(\theta^{(k)}, \theta^*) = \frac{\theta^{(k)} \cdot \theta^*}{||\theta^*|| \cdot ||\theta^{(k)}||} = \left( \frac{\theta^{(k)} \cdot \theta^*}{||\theta^*||} \right) \cdot \left( \frac{1}{||\theta^{(k)}||} \right)$$

**Step 1: 분자(첫 번째 괄호) 분석**
학습이 한 번 일어날 때마다($k$번째 실수), 분자는 최소 $\gamma$만큼 커집니다.
$$\frac{\theta^{(k)} \cdot \theta^*}{||\theta^*||} = \frac{(\theta^{(k-1)} + y^{(i)}x^{(i)}) \cdot \theta^*}{||\theta^*||} = \frac{\theta^{(k-1)} \cdot \theta^*}{||\theta^*||} + y^{(i)} \frac{x^{(i)} \cdot \theta^*}{||\theta^*||} \ge \frac{\theta^{(k-1)} \cdot \theta^*}{||\theta^*||} + \gamma$$
수학적 귀납법에 의해, $k$번 실수하면 이 값은 **$k\gamma$** 이상이 됩니다.

**Step 2: 분모(두 번째 괄호) 분석**
틀렸을 때만 업데이트하므로 $y^{(i)}(\theta^{(k-1)T}x^{(i)}) \le 0$입니다. 이를 이용해 벡터의 크기를 계산하면:
$$||\theta^{(k)}||^2 = ||\theta^{(k-1)} + y^{(i)}x^{(i)}||^2 = ||\theta^{(k-1)}||^2 + 2y^{(i)}\theta^{(k-1)T}x^{(i)} + ||x^{(i)}||^2$$
가운데 항이 0 이하이므로, $||\theta^{(k)}||^2 \le ||\theta^{(k-1)}||^2 + R^2$가 됩니다. 역시 귀납법으로 $k$번 실수하면 $||\theta^{(k)}||^2 \le kR^2$, 즉 **$||\theta^{(k)}|| \le \sqrt{k}R$**입니다.

**Step 3: 결합**
이제 코사인 식에 대입합니다:
$$\cos(\theta^{(k)}, \theta^*) \ge (k\gamma) \cdot \frac{1}{\sqrt{k}R} = \sqrt{k} \cdot \frac{\gamma}{R}$$
코사인 값은 절대로 **1을 넘을 수 없습니다**.
$$1 \ge \sqrt{k} \cdot \frac{\gamma}{R} \implies 1^2 \ge k \cdot \frac{\gamma^2}{R^2} \implies k \le \left( \frac{R}{\gamma} \right)^2$$

### 결론
이 수학적 증명은 퍼셉트론이 무한 루프에 빠지지 않고, 데이터의 범위($R$)와 마진($\gamma$)에 의해 결정되는 **최대 실수 횟수 이내에 반드시 학습을 끝낸다**는 사실을 보장합니다.



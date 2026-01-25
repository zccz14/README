---
"title": "Análisis de Espectro Completo: El Método Óptimo para Monetizar Información"
"summary": "Este artículo presenta el Análisis de Espectro Completo (FSA), un marco de estrategia de inversión y trading optimizado basado en el Criterio de Kelly. Primero, analiza las limitaciones de la fórmula tradicional de Kelly en aplicaciones de inversión, como la falta de consideración de apalancamiento y ventas en corto, problemas de timing de liquidación, etc. Luego, el FSA construye un modelo sistemático de decisión de trading definiendo el espacio de resultados, calculando el apalancamiento óptimo y la tasa de rendimiento compuesta. El artículo detalla los principios matemáticos del FSA, incluyendo el cálculo del rendimiento esperado y compuesto, así como el algoritmo para resolver el apalancamiento óptimo usando el método de Newton. Además, introduce métodos de backtesting histórico (como el cálculo del Margen Bruto de Beneficio, GPM), consideraciones para el módulo de trading en tiempo real y medidas para eventos de cisne negro. La ventaja central del FSA radica en su capacidad para utilizar información probabilística imperfecta, optimizando la decisión de apalancamiento para maximizar el rendimiento a largo plazo, reduciendo así los altos requisitos de calidad de la información."
"tags":
  - "Análisis de Espectro Completo"
  - "Fórmula de Kelly"
  - "Estrategia de Inversión"
  - "Optimización de Apalancamiento"
  - "Tasa de Rendimiento Compuesta"
  - "Gestión de Riesgos"
  - "Trading Algorítmico"
  - "Eventos de Cisne Negro"
"date": "2025-08-10"
---

# Análisis de Espectro Completo: El Método Óptimo para Monetizar Información

2025-08-10

## Antecedentes

**¿Cómo maximizar la tasa de crecimiento del capital?**

En 1956, J.L. Kelly publicó "A New Interpretation of Information Rate" (Una Nueva Interpretación de la Tasa de Información). El artículo discute cómo un apostador puede utilizar información conocida para maximizar la tasa de crecimiento compuesta de su capital. Propone un método probabilístico para guiar la estrategia de apuestas. Sin embargo, aplicar este artículo a actividades de inversión reales presenta varios problemas:

1.  No utiliza apalancamiento mediante préstamo ni ventas en corto, por lo que sus conclusiones se mantienen dentro del rango de apalancamiento real de 0 a 1.
2.  Asume que el apostador apuesta por un símbolo de evento, recibiendo un pago si ocurre la próxima vez, o perdiendo su apuesta en caso contrario. Pero un trader solo puede apostar a largo/corto, y los eventos alcistas/bajistas correspondientes presentan resultados y rendimientos diferentes en diferentes ventanas de tiempo.
3.  *Timing* de liquidación: Kelly asume que el final y la liquidación de la apuesta no están determinados por la voluntad del apostador. Pero un trader puede decidir salir o mantener la posición en cualquier momento.
4.  Método bayesiano redundante, que asume que el desarrollo de eventos está relacionado con probabilidades previas y condicionales anteriores. Eliminar el método bayesiano no afecta la esencia matemática derivada. Aumenta la dificultad, haciendo que el uso directo de la fórmula del artículo sea complejo y requiera simplificación para su uso práctico.

Continuaremos utilizando el método probabilístico del artículo de Kelly para explorar una estrategia práctica aplicable al campo del trading de inversión.

## Análisis de Espectro Completo: Full-Spectrum Analysis, FSA

### Espacio de Resultados, Apalancamiento Óptimo y Rendimiento Óptimo

Supongamos que al implementar una estrategia de trading, eventualmente habrá varios eventos de resultado diferentes. Sea el espacio de resultados $X$. $X$ no está vacío (en escenarios reales, existen al menos 2 resultados).

Hacemos algunas suposiciones adicionales:

1.  Rendimiento Determinista: Cada evento de resultado corresponde a una tasa de rendimiento definida. Sea $R_i$ la tasa de rendimiento del resultado $i$.
    1.  ✅ Eventos con diferentes tasas de rendimiento deben considerarse eventos de resultado diferentes, por ejemplo, una ganancia del 1% y una ganancia del 2% son diferentes.
    1.  ❌ Agrupar eventos de ganancia con rendimientos claramente diferentes en el mismo resultado viola el rendimiento determinista.
1.  Probabilidad Determinista: Sea $P_i$ la probabilidad de que ocurra el resultado $i \in X$, ($0 \le P_i \le 1$).
    1.  ✅ La probabilidad de cada resultado debe evaluarse de manera determinista.
    1.  ❌ Considerar que la posibilidad de un resultado es "más del 80%": 80%～ 100%, viola la probabilidad determinista.
1.  Exclusividad Mutua: No es posible que ocurran 2 resultados simultáneamente.
    1.  ✅ Se deben diseñar eventos de resultado que sean completamente mutuamente excluyentes.
    1.  ❌ Si los eventos de resultado tienen intersección, por ejemplo, los eventos "mantener posición durante 20 minutos" y "caída del 20%" podrían ocurrir simultáneamente, violando la exclusividad mutua.
1.  Exhaustividad: La suma de las probabilidades de todos los resultados es del 100%, no existen resultados fuera de los resultados predefinidos, es decir, $\sum_{i \in X} P_i = 1$.
    1.  ✅ Se deben considerar todos los resultados posibles.
    1.  ❌ Si los eventos de resultado solo definen "subida >20%", "bajada >20%", pero no definen "movimiento entre -20% y 20%", se viola la exhaustividad.
1.  Efecto de Apalancamiento: La tasa de rendimiento sigue el efecto de apalancamiento, es decir, si se usa un apalancamiento de $k$, cuando ocurre el resultado $i$, la tasa de rendimiento es $k \cdot R_i$, lo que significa que el capital se convierte en $1 + k \cdot R_i$ veces el original.
1.  ✅ La mayoría de los instrumentos de inversión con suficiente liquidez cumplen esta característica.
1.  ❌ Cuando el tamaño de la posición es demasiado alto, debido a problemas de liquidez, se generan costos de impacto de mercado y el rendimiento ya no sigue una relación lineal.

Para el espacio de resultados $X$, podemos calcular su **Rendimiento Esperado (Expected Earning Yield)**:
$$E(X) = \sum_{i \in X} P_i R_i$$

El concepto de rendimiento esperado es bastante intuitivo y fácil de calcular. Los problemas del rendimiento esperado son:

1.  Conduce a decisiones extremas, ya sea posición cero o posición máxima, sin poder controlar el riesgo de ruina. Si el rendimiento esperado es positivo, tenderá a ampliar el apalancamiento infinitamente para buscar un mayor rendimiento, incluso si un alto apalancamiento puede llevar a la ruina.
2.  No considera el efecto del interés compuesto. Solo considera el rendimiento de una sola inversión, no considera que el mercado permite inversiones repetidas. No se ajusta a la realidad de la gestión de inversiones.

**Rendimiento Compuesto (Compound Earning Yield)**, se refiere a la tasa de rendimiento promedio por operación después de realizar múltiples operaciones independientes repetidamente:

$$C(X, k) = (\prod_{i \in X} (1 + k \cdot R_i)^{P_i}) - 1$$

**Rendimiento Óptimo (Optimal Earning Yield)**, es el valor máximo del rendimiento compuesto bajo diferentes niveles de apalancamiento:

$$O(X) = \max_k C(X, k)$$

Para evitar la ruina (llegar a cero), se debe satisfacer que para todos los resultados, $1 + k \cdot R_i >0$, lo que conduce a un dominio factible básico $K$ para k:

Límite superior:

$$\max k = \max(0, \min_{R_i < 0} {-\frac{1}{R_i}})$$

Límite inferior:

$$\min k = \min(0, \max_{R_i > 0} {-\frac{1}{R_i}})$$

Y una solución siempre factible $k = 0$,

$$K = (\min k, \max k) \cup \{ 0 \}$$

El apalancamiento óptimo es el $k$ dentro del dominio factible $K$ que maximiza el rendimiento compuesto $C(X, k)$.

$$k_o = \argmax_k C(X, k)$$

En cualquier momento, se puede calcular el apalancamiento real de la cuenta: $k = \frac{Tamaño\ de\ la\ posición}{Valor\ neto\ de\ la\ cuenta}$, y también evaluar el apalancamiento óptimo: $k_o$.
Todas las estrategias de trading eventualmente corresponden a un apalancamiento real. Tomar una decisión de trading determinista equivale a decidir el valor de $k$. Y el apalancamiento óptimo corresponde a la posición ideal.

Así obtenemos un marco de trading:

1.  Diseñar el espacio de resultados $X$.
2.  Evaluar continuamente las probabilidades $P$ de diferentes resultados.
3.  Calcular el apalancamiento real $k$ y el apalancamiento óptimo $k_O$.
4.  Controlar el apalancamiento real para que se acerque al apalancamiento óptimo.

> **Puntos Especiales**
>
> Punto trivial $C(X, 0) = 0$, es decir, "sin participación no hay ganancias ni pérdidas".
> Cuando no tenemos posición, es equivalente a decidir $k = 0$. Para cualquier instrumento, con o sin posición, estamos tomando una decisión en cada momento.
> Venta en corto corresponde al caso $k < 0$; si $|k| > 1$, significa que se necesita apalancamiento adicional.

### Resolviendo la Optimización

Revisando el problema, conocido el espacio de resultados $X$, la distribución de probabilidad $P_i$, el rendimiento $E_i$, el dominio factible $[L, R]$, encontrar la tasa de apalancamiento $k \in [L, R]$ que maximiza el rendimiento:

Reorganizando el rendimiento compuesto, tomando logaritmo en ambos lados (preservando la monotonicidad), obtenemos:

$$\ln (1 + C(X, k)) = \sum_{i \in X} P_i \ln (1 + k \cdot R_i)$$

Sea

$$G(k) = \ln(1 + C(X, k)) = \sum_{i \in X} P_i \ln (1 + k R_i)$$

Según la definición de apalancamiento óptimo:

$$k_o = \argmax_k C(X, k)$$

Dado que $f(x) = \ln(1+x)$ es monótonamente creciente:

$$k_o = \argmax_k C(X, k) = \argmax_k G(k)$$

Calculando la primera y segunda derivada de $G(k)$:

$$G'(k) = \sum_{i \in X} \frac{P_i R_i}{1 + k R_i} $$

$$G''(k) = \sum\_{i \in X} -\frac{P_i R_i^2}{(1 + k R_i)^2} $$

La segunda derivada es no positiva en cada término, por lo tanto $G''(k) \le 0$. Solo cuando $P_i R_i \equiv 0$, $G''(k) = 0$, y en ese caso, $G(k) \equiv 0$, no hay rendimiento, carece de significado práctico. En otros casos, $G''(k) < 0$. Esto significa que $G(k)$ es una función estrictamente cóncava, $G'(k)$ es estrictamente monótona decreciente. $G(k)$ tiene un único punto máximo, y este punto máximo es también el punto de valor máximo.

Como se muestra en la siguiente figura, sin importar cuántos términos tenga $G(k)$, su curva es cóncava.

![Ilustración de la curva de la Fórmula de Kelly](image-1.png)

El punto máximo de $G(k)$ es el cero de su primera derivada:

$$G'(k) = \sum_{i \in X} \frac{P_i R_i}{1 + k R_i} = 0$$

Si reorganizamos esta ecuación, se puede transformar en una ecuación polinómica en k.
Si existen $N$ valores diferentes para $R_i$, el grado máximo del polinomio resultante es $N-1$.
Según el teorema de Abel-Ruffini, polinomios de grado cinco o superior no tienen una fórmula general para sus raíces.

La fórmula de Kelly común es un caso especial con $N=2$: Sea $p$ la probabilidad de ganar, $b$ la cuota de ganancia.

| Resultado | Probabilidad | Rendimiento |
| --------- | ------------ | ----------- |
| Ganar     | $p$          | $b$         |
| Perder    | $1-p$        | $-1$        |

Sustituyendo obtenemos la ecuación:

$$G'(k) = \frac{pb}{1+kb} + \frac{-(1-p)}{1 -k} = 0$$

Reorganizando se obtiene

$$k = \frac{p(b+1)-1}{b}$$

Que es la fórmula de Kelly.

Para escenarios de trading, pueden existir infinitos resultados con diferentes rendimientos, por lo tanto, solo se pueden buscar soluciones numéricas.

Como función estrictamente cóncava de una variable, usar el método de Newton es la opción ideal.
Comenzar la iteración desde el punto fijo $$G(0) = 0$$ es una buena opción; una función estrictamente cóncava convergerá al mismo resultado desde cualquier punto inicial.

#### Caso donde el Método de Newton sale del Dominio Factible

Un problema potencial es que el punto de iteración obtenido por el método de Newton puede estar fuera del dominio factible del problema.
Usemos un ejemplo mínimo para ilustrar esta situación:

$$p_1 = 0.9, r_1 = 0.5, p_2 = 0.1, r_2 = -1$$

Obtenemos

$$G(k) = 0.9 \times \ln(1+0.5k) + 0.1 \times \ln(1 - k)$$

![Imagen de la función G(k)](image-2.png)

Cálculo muestra que el dominio factible $K = (-2, 1)$, el apalancamiento óptimo analítico es $k_o = 0.7$,
pero al usar el método de Newton, el primer punto de iteración

$$G(0) - \frac{G'(0)}{G''(0)} = \frac{14}{13} > 1$$

![Demostración de G'(k) y el método de Newton](image-3.png)

El primer punto de iteración ya está fuera del dominio factible, fuera del dominio de definición de la derivada, y continuar iterando carece de sentido. Esto muestra que el método de Newton ingenuo por sí solo no puede manejar situaciones donde se sale del dominio factible.

La solución es, después de calcular el punto de iteración usando el método de Newton, verificar adicionalmente si está dentro del dominio factible. Si lo está, iterar hasta allí; si no, según su dirección, tomar un punto entre el límite del dominio factible y el punto actual como el siguiente punto de iteración.

#### Pseudocódigo del Algoritmo

Algoritmo $\text{resolve}(G, L, R, \epsilon = 10^{-9}, N = 100, \alpha = 0.9)$

1.  Inicializar $k \gets 0$
2.  Según las tasas de rendimiento $r_i$, calcular el dominio factible básico $K$
3.  Recortar el dominio factible $L \gets \max(L, \min K), R \gets \min(R, \max K)$
4.  Bucle máximo $N$ veces:
    1.  Calcular el siguiente punto $k' \gets k - \frac{G'(k)}{G''(k)}$
    2.  Si $k'$ no pertenece a $K$
        1.  Si $k' \ge R$, tomar $k' \gets \alpha R + (1-\alpha)k$
        2.  Si $k' \le L$, tomar $k' \gets \alpha L + (1 - \alpha) k$
    3.  Si la diferencia es menor que el umbral de precisión $| k' - k | < \epsilon$, salir del bucle.
    4.  Actualizar $k \gets k'$
5.  Devolver $k$

#### Implementación del Código

Desde una perspectiva de programación, una abstracción más adecuada es: supongamos que cada resultado en el conjunto tiene dos atributos, tasa de rendimiento r y peso w. Este espacio de resultados puede ser recorrido, entonces el algoritmo puede escribirse como:

```ts
/**
 * Según el Criterio de Kelly, calcula el apalancamiento óptimo k y el rendimiento esperado e.
 *
 * @param R - Vector de tasas de rendimiento
 * @param W - Vector de pesos
 * @param lower - Límite mínimo de apalancamiento
 * @param upper - Límite máximo de apalancamiento
 * @param eps - Precisión de convergencia
 * @param max_iter - Número máximo de iteraciones
 * @param alpha - Factor de aceleración de convergencia
 * @returns Un objeto que contiene el apalancamiento óptimo k y el rendimiento esperado e
 */
export function resolve_k(
  R: number[],
  W: number[],
  lower = -Infinity,
  upper = Infinity,
  eps = 1e-9,
  max_iter = 100,
  alpha = 0.9
) {
  const n = R.length;
  if (n !== W.length)
    throw new Error(
      "Los vectores de Rendimiento y Probabilidad deben tener la misma longitud"
    );

  // Calcular el dominio factible básico K, tal que 1 + k * r > 0
  let minK = NaN;
  let maxK = NaN;
  for (let i = 0; i < n; i++) {
    const r = R[i];
    if (r === 0) continue;
    const k = -1 / r; // Valor crítico
    if (r > 0) minK = isNaN(minK) ? k : Math.max(minK, k);
    if (r < 0) maxK = isNaN(maxK) ? k : Math.min(maxK, k);
  }
  if (isNaN(minK)) minK = 0; // Si no hay R positivos, minK toma 0
  if (isNaN(maxK)) maxK = 0; // Si no hay R negativos, maxK toma 0
  lower = Math.max(lower, minK);
  upper = Math.min(upper, maxK);

  let sum_w = 0;
  for (let i = 0; i < n; i++) {
    const w = W[i];
    if (w < 0) throw new Error(`Weight[${i}] = ${w} debe ser no negativo`);
    sum_w += w;
  }
  if (sum_w === 0) throw new Error("La suma de los pesos debe ser mayor que cero");

  let k = 0;
  let it;
  for (it = 0; it < max_iter; it++) {
    let acc_g1 = 0;
    let acc_g2 = 0;
    for (let i = 0; i < n; i++) {
      const r = R[i];
      const w = W[i];
      acc_g1 += (w * r) / (1 + k * r);
      acc_g2 += (w * r * r) / (1 + k * r) ** 2;
    }
    const delta_k = acc_g1 / acc_g2;
    if (!(Math.abs(delta_k) > eps)) break;
    let next_k = k + delta_k;
    if (next_k <= lower) {
      next_k = lower * alpha + k * (1 - alpha);
    } else if (next_k >= upper) {
      next_k = upper * alpha + k * (1 - alpha);
    }

    k = next_k;
  }

  const lne =
    R.reduce((acc, r, i) => acc + W[i] * Math.log(1 + k * r), 0) / sum_w;
  const e = Math.exp(lne) - 1;

  return { k, e, it, sum_w, lne, upper, lower };
}
```

### Otras Propiedades Matemáticas

#### Sin restricciones de dominio factible, cuando el rendimiento esperado es positivo, el rendimiento óptimo es positivo

Demostración:
Dado que $f(x) = \ln(1+x)$ tiene el mismo signo que $x$, el signo de $C(X, k) $ y $G(k)$ es el mismo,
consideremos la derivada de $G(k)$ en $k = 0$: $G'(0) = \sum_{i \in X} P_i R_i $, que en realidad es el rendimiento esperado $E(X)$.
Para una cantidad infinitesimal $\Delta k$, según la definición de derivada, $G(\Delta k) = G(0) + G'(0) \cdot \Delta k = E(X) \cdot \Delta k$

Si $E(X) > 0$, entonces existe $\Delta k > 0$ tal que $G(\Delta k) > 0$, es decir, $C(X, \Delta k) >0$, y el rendimiento óptimo
$$O(X) = \max C(X, k) \ge C(X, \Delta k) > 0$$

Q.E.D.

Además, se puede demostrar la siguiente tabla:

| Rendimiento Esperado | Apalancamiento Óptimo | Rendimiento Óptimo |
| -------------------- | --------------------- | ------------------ |
| Positivo             | Positivo              | Positivo           |
| 0                    | 0                     | 0                  |
| Negativo             | Negativo              | Positivo           |

## Método de Backtesting Histórico para FSA

### Margen Bruto de Beneficio: Gross Profit Margin, GPM

Según el marco de trading descrito, en cada momento se puede calcular el apalancamiento real $k$ y el apalancamiento óptimo $k_O$, y controlar que el apalancamiento real se acerque al óptimo.

Aquí **asumimos por defecto un método de backtesting de interés simple**, porque el modelo de interés compuesto afectaría las estimaciones de costos posteriores. El modelo de interés compuesto generaría cambios muy grandes en el volumen negociado, lo que llevaría a que, una vez alcanzada la capacidad de la estrategia, se produzcan costos de impacto de mercado adicionales, haciendo que el volumen realmente negociable o los costos se desvíen severamente de los valores del modelo, distorsionando el backtesting. En escenarios reales, la operación de interés compuesto a menudo es controlada subjetivamente, es decir, ajustando subjetivamente el valor neto inicial o el multiplicador de trading para producir un esquema de "interés parcialmente compuesto" entre "interés simple" e "interés compuesto".

Restricción clave: $K_t$ depende solo de la información conocida en los momentos $0, 1, ..., t$, no hay función de datos futuros. $K_t$ afecta el tamaño de la posición en el momento $t+1$.

Para realizar backtesting histórico, primero necesitamos conocer el precio $P_t$, y el volumen neto de posición planificado correspondiente $V_t$.

A nivel micro, en el momento $t$, al conocer el precio $P_t$, también conocemos el valor neto $E_t$ y el volumen neto de posición $H_t$.

Consideremos primero el caso límite: $V_0 = 0, E_0 = 0$.

Después de un tiempo de análisis despreciable, obtenemos el volumen neto de posición planificado $V_t$.

El volumen de trading necesario para ajustar la posición es $V_t - H_t$

Posteriormente, hasta el momento $t + 1$, se comenzará a realizar órdenes inmediatamente.

Bajo el supuesto de liquidez suficiente, se ejecutará completamente al precio $P_{t+1}$, haciendo que $H_{t+1} = V_t$.
Sea $c$ el costo basado en el volumen negociado, entonces el costo es $c \cdot |V_t - H_t| \cdot P_{t+1}$.
Además, la posición neta $H_t$ se ve afectada por el cambio de precio, generando ganancias/pérdidas $H_t (P_{t+1} - P_t)$.

En resumen, en el momento $t + 1$:

$$H_{t+1} = V_t$$
$$E_{t+1} = E_t + H_t (P_{t+1} - P_t) - c \cdot |H_{t+1} - H_t| \cdot P_{t+1}$$

En el modo de interés simple, el tamaño de la posición y los costos de trading son proporcionales al valor neto inicial.

Después de tomar una posición, la ganancia/pérdida total (Profit and Loss, PnL) por cambios de precio:

$$\text{PnL} = \sum_t H_t \cdot (P_{t + 1} - P_t)$$

Volumen total negociado:

$$\text{Turnover} = \sum_t |H_{t+1} - H_t| \cdot P_{t+1}$$

Se puede estimar el costo máximo de trading que el modelo puede soportar para alcanzar el punto de equilibrio, es decir, el Margen Bruto de Beneficio (Gross Profit Margin, GPM):

$$\text{GPM}  = \frac{\text{PnL}}{\text{Turnover}}$$

Posteriormente, en el trading en tiempo real, una tasa de costo real por debajo de este GPM será rentable. Este GPM sugiere la capacidad del modelo; si este GPM es relativamente alto, significa que en tiempo real se pueden usar *slippages* de trading más grandes, aumentando el volumen real negociado.

La tarea del modelo es maximizar el GPM, y la tarea del módulo de trading es lograr ganancias bajo esta restricción de GPM. Específicamente, en el trading posterior en tiempo real, la tarea del módulo de trading es completar la mayor cantidad de volumen negociado posible sin exceder el GPM. El módulo de trading no puede evitar las tasas de comisión inherentes al exchange, que pueden verse afectadas por varios factores, como VIP, reembolsos, Maker/Taker, etc., lo que afecta la tasa de comisión real. Si el GPM del modelo es mayor que la tasa de comisión de un exchange, se puede considerar que es difícil que el modelo sea rentable en ese exchange, y necesita mejoras. Si el módulo de trading considera que la tarea actual no se puede lograr, puede optar por reducir el volumen negociado o no operar, manteniendo una posición cero.

La ganancia final puede considerarse como: Volumen negociado \* (GPM - costo real). Si se aumenta el valor neto inicial, aumentará el volumen negociado, haciendo que la tasa de costo real se acerque continuamente al GPM, hasta que no sea rentable. Pero según la fórmula, debería existir un problema de optimización de la ganancia. El valor neto inicial correspondiente a esta ganancia máxima es la capacidad del modelo de trading. La evaluación específica requiere un estudio más profundo de la relación entre el volumen negociado y el costo.

### Resolución de la Posición

En el trading real, los productos tienen un paso mínimo de volumen negociable (*volume_step*), las posiciones solo pueden negociarse en múltiplos enteros de este paso.
Por lo tanto, dado un objetivo de posición en número flotante, no se puede seguir este objetivo al 100%. Por lo tanto, necesitamos redondear este objetivo de posición a una posición negociable.

La Resolución de la Posición, *Holding Resolution*, es un número entero positivo.

Si para el apalancamiento óptimo $k_O$ se aplica el método de interés simple, y luego se mapea mediante la resolución, se obtiene la posición objetivo $V$. Sustituyendo en el marco de backtesting se puede calcular el MER.

-   Si la resolución de la posición = 1, significa que la estrategia solo operará la posición base. Es decir, los valores de la posición objetivo son -1, 0, 1.
-   Si la resolución de la posición = 2, significa que la estrategia comienza a necesitar dividir la posición. Los valores de la posición objetivo son -2, -1, 0, 1, 2.
-   Si la resolución de la posición = ∞, significa que la estrategia puede ajustar la posición con cualquier precisión. Pero esto no se ajusta a la realidad.

Cuanto menor sea la resolución de la posición, menor será el capital base necesario para el posterior trading en tiempo real con capital pequeño, pero la información utilizada será más borrosa.

Teóricamente, la resolución de la posición afecta el volumen negociado; a menor resolución, menor volumen negociado (las situaciones que requerían ajuste de posición se convierten en no requeridas). El impacto de la resolución de la posición en el rendimiento no está claro. Empíricamente, si el MER es lo suficientemente alto y no es sensible a la resolución, significa que se puede proceder directamente al trading en tiempo real.

## Módulo de Trading en Tiempo Real

El módulo de trading en tiempo real necesita lograr ganancias bajo la restricción del MER.

Pero las restricciones para abrir y cerrar posiciones no son consistentes; al abrir se puede tolerar que el volumen objetivo no se complete completamente, pero al cerrar no se puede tolerar esto. Por lo tanto, al cerrar, las restricciones son más estrictas; en el peor caso, se debe operar con órdenes de mercado, lo que generará mayores comisiones y *slippage*.

Supongamos que la tasa de costo de una orden de mercado es $c$, entonces al abrir se necesita operar con una tasa de costo de $2 \times \text{MER} - c$ para estar seguro.

Por ejemplo, MER = 0.02%, costo de orden de mercado = 0.03%, entonces el costo de apertura debe ser como máximo 0.01%.

## Sobre el Espacio de Resultados

### Medidas para Cisnes Negros

Un cisne negro es un evento extremadamente improbable que, sin embargo, ocurre.

1.  Es imposible estimar la probabilidad de un cisne negro a través de cualquier modelo; cualquier estimación de un cisne negro es inútil, su probabilidad es incognoscible.
2.  Una vez que ocurre un evento de cisne negro, generará pérdidas enormes.
    Por lo tanto, al diseñar cualquier espacio de resultados, es necesario defenderse de los eventos de cisne negro.
3.  Cuando ocurre un cisne negro, inevitablemente genera un rendimiento del -100%, es decir, pérdida total.
4.  Un cisne negro no puede ser ajustado por ninguna distribución de probabilidad utilizando muestras conocidas.
    Por lo tanto, es necesario inventar una pseudoprobabilidad para el evento de cisne negro.

Supongamos que a partir de muestras existentes, asignamos una probabilidad $P_i$ a nuestro espacio de resultados definido $X$.

Necesitamos agregar artificialmente dos eventos de cisne negro simétricos: $P_b^- = 0.0013, r_b = -1, P_b^+ = 0.0013, r_b = 1$. Donde 0.0013 es la probabilidad fuera de $3\sigma$ en una distribución normal, aproximadamente 1 en 770 muestras. Cuanto mayor sea la probabilidad asignada a los eventos de cisne negro, más conservadora será la estrategia.

Diseñar eventos de cisne negro simétricos es para no afectar el rendimiento esperado, evitar cambiar el signo del apalancamiento óptimo y evitar que en situaciones donde originalmente el apalancamiento era 0, se juzgue erróneamente que se necesita vender en corto.

Las probabilidades originales deben reducirse a $1 - P_b^- - P_b^+ = 0.9974$ veces, para garantizar la exhaustividad de todo el nuevo espacio de resultados.

Agregar eventos de cisne negro reducirá el dominio factible, la tasa de apalancamiento óptimo estará estrictamente limitada a $(-1, 1)$, aún se puede vender en corto, pero no se puede agregar apalancamiento adicional. La inclusión de eventos de cisne negro puede prevenir efectivamente el problema del uso excesivo de apalancamiento.

```ts
export function withBlackSwan(R: number[], W: number[], Pb = 0.0013) {
  const sum_w = W.reduce((acc, cur) => acc + cur, 0);
  const w_b = (Pb * sum_w) / (1 - 2 * Pb);
  return {
    R: R.concat([1, -1]),
    W: W.concat([w_b, w_b]),
  };
}
```

## Resumen

Para un espacio de resultados dado $X$, $R_i$ es determinista; siempre que se pueda estimar la distribución de probabilidad $P_i$ en el espacio de resultados, se puede obtener el apalancamiento óptimo determinista $k_O$. Siempre que se considere que el sistema de trading debe ser consistente, sus probabilidades deberían ser repetibles. En este contexto, el Análisis de Espectro Completo utiliza información imperfecta sin pérdidas, por lo que no hay ninguna razón para no seguir estrictamente este apalancamiento óptimo.

Algunos sistemas de trading intentan encontrar el resultado de mayor probabilidad y formular un plan de trading basado en ese resultado. Este es un método de máxima verosimilitud. El riesgo de este método es que si la función de verosimilitud es relativamente plana, elegir cualquier interpretación individual no es lo suficientemente precisa. Esta estrategia parecerá efectiva a veces e inefectiva otras. El Análisis de Espectro Completo no necesita seguir el resultado de mayor probabilidad; puede calcular el rendimiento bajo diferentes resultados y seleccionar la posición óptima. Puede capturar información sutil y tomar la decisión óptima. Por lo tanto, el Análisis de Espectro Completo reduce enormemente el umbral de calidad requerido para monetizar información.

En cuanto a cómo diseñar el espacio de resultados y estimar la distribución de probabilidad, pertenece al contenido de la información misma que necesita ser monetizada, lo cual se discutirá en una próxima ocasión.
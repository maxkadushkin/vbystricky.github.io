---
layout: post
title: Задача о "многоруком бандите" (часть 3)
date: 2017-02-25
category: Reinforcement Learning
tags: [ML, RL, multi-arms bandit]
use_math: true
---

Разберем еще одну стратегию решения задачи о "многоруком бандите".

<!--more-->

### Стратегия верхнего доверительного интервала

В жадной стратегии мы на каждом шаге выбирали ручку, для которой оценка математического ожидания награды на этом шаге максимальна. Мы уже разобрались
почему это не лучшая стратегия, и как её можно улучшить при помощи "оптимистичной инициализации". Но мы не всегда обладаем априорными знаниями, чтобы
"правильно" инициализироваться. Поэтому трюк с "оптимистичной инициализацией" не всегда применим.

$\epsilon$-жадная стратегия показывает лучшие результаты, чем просто жадная стратегия (если мы не используем "оптимистичную инициализацию") за счет
того, что время от времени пытается тестировать ручки для которых текущая оценка не максимальна. Это позволяет не застрять в локальном минимуме.
Однако, минус $\epsilon$-жадной стратегии в том, что ручка для тестирования выбирается каждый раз случайным образом, без учета уже имеющейся
информации. А хотелось бы тестировать ручки, которые либо выглядят более привлекательными (т.е. имеют достаточно большую текущую оценку
математического ожидания), либо ещё не достаточно исследованы. В качестве улучшенной $\epsilon$-жадной стратегии, отвечающей таким требованиям можно
использовать стратегию верхнего доверительного интервала (*Upper Confidence Bound*). Согласно этой стратегии на шаге $t$ выбираем действие $a_t$ 
следующим образом:

$$a_t = argmax_{a=1,...,N}\left\{Q_a + c\sqrt{\frac {ln(t)} {P_a} }\right\}$$

здесь сохранены обозначения, использованные раньше:

+ $P_a$ - сколько раз было выбрано действие $a$

+ $Q_a$ - текущая оценка математического ожидания награды для действия $a$

Сравним новую стратегию с $\epsilon$-жадной в рамках эксперимента, описанного в 
[первой части]({% post_url 2017-01-28-rl_multi_arms_bandits %}#Эксперимент)

![График сравнение e-жаднщй и UCB стратегий]({{ site.baseurl }}/images/2017-02/rl_multi_arms_bandits_p3_1.png)

Из графика видно, что для разных коэффициентов $c$ можно получить как вариант лучше так и хуже $\epsilon$-жадной стратегии. Важно учитывать, что 
эксперимент мы проводим для определенных рапределений наград на ручках бандита и это сказывается на том какие параметры $\epsilon$ и $c$ оптимальны
для стратегий.

Код можно найти [здесь](https://github.com/vbystricky/vbystricky_tests/tree/master/multi_arms_bandits). 

---

#### Литература:

+ [Sutton and Barto. An introduction to Reinforcement Learning.](http://webdocs.cs.ualberta.ca/~sutton/book/the-book.html)


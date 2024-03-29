---
## Front matter
lang: ru-RU
title: Лабораторная работа №5
subtitle: Модель хищник-жертва
author:
  - Камкина А. Л.
institute:
  - Российский университет дружбы народов, Москва, Россия

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
---

# Информация

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Камкина Арина Леонидовна
  * студентка
  * Российский университет дружбы народов
  * [1032216456@pfur.ru](mailto:1032216456@pfur.ru)
  * <https://alkamkina.github.io/ru/>

:::
::: {.column width="25%"}

![](./image/me.jpg)

:::
::::::::::::::


## Цель работы

Исследовать модель взаимодействия двух видов типа «хищник — жертва» -
модель Лотки-Вольтерры и построить графики, используя языки Julia и OpenModelica.

## Моде́ль Ло́тки — Вольте́рры

$Моде́ль Ло́тки — Вольте́рры$ — модель взаимодействия двух видов типа «хищник — жертва», названная в честь своих авторов (Лотка, 1925; Вольтерра 1926), которые предложили модельные уравнения независимо друг от друга.[1]

Простейшая модель взаимодействия двух видов типа «хищник — жертва» -
модель Лотки-Вольтерры. Данная двувидовая модель основывается на
следующих предположениях:
1. Численность популяции жертв x и хищников y зависят только от времени
(модель не учитывает пространственное распределение популяции на
занимаемой территории)
2. В отсутствии взаимодействия численность видов изменяется по модели
Мальтуса, при этом число жертв увеличивается, а число хищников падает
3. Естественная смертность жертвы и естественная рождаемость хищника
считаются несущественными
4. Эффект насыщения численности обеих популяций не учитывается
5. Скорость роста численности жертв уменьшается пропорционально
численности хищников

## Моде́ль Ло́тки — Вольте́рры
$$\begin{cases}
\dfrac{dx}{dt} = -a x(t) + b x(t)y(t)\\
\dfrac{dy}{dt} = c y(t) - d x(t)y(t)
\end{cases}$$ 

В этой модели $x$ – число жертв, $y$ - число хищников. Коэффициент $a$
описывает скорость естественного прироста числа жертв в отсутствие хищников, $с$ - естественное вымирание хищников, лишенных пищи в виде жертв. Вероятность
взаимодействия жертвы и хищника считается пропорциональной как количеству
жертв, так и числу самих хищников ($xy$). Каждый акт взаимодействия уменьшает
популяцию жертв, но способствует увеличению популяции хищников (члены $-bxy$
и $dxy$ в правой части уравнения). 
Стационарное состояние системы (положение равновесия, не зависящее
от времени решение) будет в точке:
$x0=c/d$ $y0=a/b$.

# Выполнение лабораторной работы
### Создание проекта (код на Julia)
```
using Plots
using DifferentialEquations

p = [0.73, 0.037, 0.52, 0.039]
u = [7.0, 16.0]
tspan = (0.0, 20.0)

function f(u, p, t)
    a, b, c, d = p
    x, y = u
    dx = -a*x+b*x*y
    dy = c*y-d*x*y
    return [dx, dy]
end

prob1 = ODEProblem(f, u, tspan, p)
sol1 = solve(prob1, Tsit5())
plot(sol1, label = ["x" "y"])
```
Полученный график(рис. @fig:001).

![График изменения численности хищников и численности жертв Julia](image/j2.png){#fig:001 width=70%}
---
Если хоти получить график при найденом стационарном состоянии, то заменяем значение $u$ на:
```
u = [0.52/0.039, 0.73/0.037]
```
Полученный график(рис. @fig:002).

![Стационарное состояние Julia](image/j1.png){#fig:002 width=70%}
---
### Создание проекта (код на OpenModelica)
```
model lab5_1
parameter Real a=0.73;
parameter Real b=0.037;
parameter Real c=0.52;
parameter Real d=0.039;
parameter Real x0=7;
parameter Real y0=16;
Real x(start=x0);
Real y(start=y0);

equation
der(x)=-a*x+b*x*y;
der(y)=c*y-d*x*y;
end lab5_1;
```
Полученный график(рис. @fig:003).

![График изменения численности хищников и численности жертв OpenModelica](image/o1.png){#fig:003 width=70%}
---
Если хоти получить график при найденом стационарном состоянии, то заменяем значение $u$ на:
```
parameter Real x0=c/d;
parameter Real y0=a/b;
```
Полученный график(рис. @fig:004).

![Стационарное состояние OpenModelica](image/o2.png){#fig:004 width=70%}
---

# Вывод
В процессе выполнения данной лабораторной работы я построила графики, используя Julia и OpenModelica, а также приобрела первые практические навыки работы с Julia и OpenModelica.
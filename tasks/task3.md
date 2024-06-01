# Лабораторная работа № З. Синтаксический разбор с использованием метода рекурсивного спуска

## Теоретическая  часть

Одним из наиболее просrых и потому одним из наиболее популярных методов нисходящего синтаксического анализа является метод рекурсивного спуска (recursive descent method). Метод основан на «зашивании» правил грамматики непосредственно в управляющие конструкции распознавателя.

Синтаксические  анализаторы,  работающие  по  методу  рекурсивного  спуска  без  возврата,  могут  быть построены для класса грамматик, называющегося `LL(1)`. Первая буква `L` в названии связана с тем, что входная цепочка читается слева направо, вторая буква `L` означает, что строится левый вывод входной цепочки, вторая `L` означает, что на каждом шаге для принятия решения используется один символ непрочитанной части входной цепочки. Для строгого определения `LL(1)` грамматики потребуются две функции - $FIRST$ и $FOLLOW$.

**Определение**. Пусть $\alpha \in (N \cup \Sigma)^+, x \in \Sigma^+, \beta \in  (N \cup \Sigma)^+$. Для КС-грамматики  $G = (N, \Sigma , Р, S)$ определена
функция

$FIRST_k(\alpha) = \textbraceleft x \space | \space \alpha \Rightarrow^* x \beta  \space и \space |x| = k \space или \space \alpha \Rightarrow^* x \space и \space |x| < k \textbraceright $

Иначе говоря, множество $FIRST_k(\alpha)$ состоит из всех терминальных префиксов длины $k$ (или меньше, если из $\alpha$ выводится терминальная цепочка длины, меньшей $k$)  терминальных цепочек, выводимых из $\alpha$. По определению полагают, что $FIRST_k( \varepsilon ) = \textbraceleft  \varepsilon \textbraceright $.


**Определение**. Пусть $\alpha, \gamma \in (N \cup \Sigma)^* , \beta \in (N \cup \Sigma)^*, x \in \Sigma^+$ . Для КС-грамматики G$G = (N, \Sigma , Р, S)$  определена
функция

$FOLLOW_k(\beta) = \textbraceleft  x \space | \space \Rightarrow^* \alpha\beta\gamma \space и \space x \in FIRST_k(\gamma) \textbraceright  \cup \textbraceleft  \varepsilon \space если \space S \Rightarrow^* \alpha\beta \textbraceright $

Иначе говоря, множество $FOLLOW_k(\beta)$ состоит из всех цепочек длины $k$ (или меньше) терминальных цепочек, которые могут встречаться непосредственно справа от $\beta$ в каких-нибудь цепочках, выводимых из аксиомы, причем если $\alpha \beta$ выводимая цепочка, то $\varepsilon$ тоже принадлежит $FOLLOW_k(\beta)$.

Для грамматики `LL(1)` $k=1$, и имеет СМЫСЛ  говорить ТОЛЬКО  о функциях $FIRST_1(\alpha)$ и $FOLLOW_1(\beta)$, а вместо фразы «терминальных цепочею> говорить «терминальных символов».

**Теорема**.  КС-грамматика  $G = (N, \Sigma, Р, S$ является `LL(1)`-грамматикой  тогда и  только тогда, когда  для каждых двух различных правил $A \rightarrow \beta$  и $А \rightarrow \gamma$ выполняется условие
$FIRST_1(\beta \space FOLLOW_1(A)) \cap FIRST_1(\gamma \space FOLLOW_1(A)) = \oslash$ при всех $A \in N$



### Алгоритм вычисления $FIRST_1$ для символов грамматики

*Вход*. Символы грамматики $Х \in (N \cup \Sigma)$. 

*Выход*. Семейство множеств $FIRST_1(X)$ 

*Метод.*

$for \space X \in (N \cup \Sigma \cup \textbraceleft \varepsilon\textbraceright ):$

$\qquad if \space X  = \varepsilon:$

$\qquad \qquad FIRST_1(X) = \textbraceleft \varepsilon\textbraceright $

$\qquad elif \space X \in \Sigma:$

$\qquad \qquad FIRST_1(x) = \textbraceleft X\textbraceright$

$\qquad else:$

$\qquad \qquad FIRST_1(X) = \oslash$

$\qquad \qquad if \space \exists (X \rightarrow \varepsilon) \in P :$

$\qquad \qquad \qquad FIRST_1(X) = FIRST_1(X) \cup \textbraceleft \varepsilon\textbraceright $

$\qquad \qquad for \space\space (X \rightarrow \alpha) \in P:$

$\qquad \qquad \qquad for \space\space (X \rightarrow \alpha) \in P:$

$\qquad \qquad \qquad \qquad let \space \alpha = Y_1Y_2...Y_k$

$\qquad \qquad \qquad \qquad for \space i \in range(1, len(\alpha) + 1):$

$\qquad \qquad \qquad \qquad  \qquad if \space \neg (Y_i \Rightarrow^* \varepsilon):$

$\qquad \qquad \qquad \qquad  \qquad \qquad FIRST_1(X) = FIRST_1(X) \cup FIRST_1(Y_1)$

$\qquad \qquad \qquad \qquad  \qquad \qquad break$

$\qquad \qquad \qquad \qquad  \qquad else:$

$\qquad \qquad \qquad \qquad  \qquad \qquad FIRST_1(X) = FIRST_1(X) \cup (FIRST_1(Y_1) \backslash \textbraceleft \varepsilon\textbraceright )$

$\qquad \qquad \qquad \qquad  \qquad \qquad continue$

$\qquad \qquad \qquad \qquad  if \space\space Y_1Y_2...Y_k \Rightarrow^* \varepsilon:$

$\qquad \qquad \qquad \qquad  \qquad FIRST_1(X) = FIRST_1(X) \cup \textbraceleft  \varepsilon \textbraceright $


**Пример**. Для $G'_0$  c правилами

$E \rightarrow T E'$

$E' \rightarrow + T E' | \varepsilon$

$T \rightarrow F T'$

$T' \rightarrow * F T' | \varepsilon$

$F \rightarrow (E) | a$

Будем иметь	

$FIRST(E) = \textbraceleft  (, а \textbraceright $

$FIRST(E') = \textbraceleft  +, \varepsilon \textbraceright $

$FIRST(Т) = \textbraceleft  (, а \textbraceright $

$FIRST(Т') = \textbraceleft  *, \varepsilon \textbraceright $

$FIRST(F) = \textbraceleft  (, а \textbraceright $


### Алгоритм вычисление FOLLOW для нетермивалов грамматики

*Вход*. Символы грамматики $A \in N$.

*Выход*. Семейство множеств $FOLLOW_1(A)$   

*Метод*.


$for \space A \in N:$

$\qquad FOLLOW_1(A) = \oslash$

$\qquad if \space A = S :$

$\qquad \qquad FOLLOW_1(A) = FOLLOW_1(A) \cup \textbraceleft  \textdollar  \textbraceright$

$\qquad for \space (A \rightarrow \gamma ) \in P:$

$\qquad \qquad if \space (A \rightarrow \alpha B \beta) \in P \wedge \beta \not= \varepsilon  \wedge \neg (\beta \Rightarrow^* \varepsilon):$

$\qquad \qquad \qquad  FOLLOW_1(B) = FOLLOW_1(B) \cup (FIRST_1(B) \backslash \textbraceleft  \varepsilon \textbraceright )$

$\qquad \qquad if \space (A \rightarrow \alpha B) \in P \vee \space (A \rightarrow \alpha B \beta) \in P  \wedge  (\varepsilon \in FIRST_1(\beta) \backslash \textbraceleft  \varepsilon \textbraceright  ) :$

$\qquad \qquad \qquad  FOLLOW_1(B) = FOLLOW_1(B) \cup FOLLOW_1(A)$

Пример. Для грамматики $G'_0$ будем иметь:

$FOLLOW(Е) = \textbraceleft  \textdollar, ) \textbraceright$

$FOLLOW (Е') = \textbraceleft  \textdollar, ) \textbraceright$

$FOLLOW (Т) = \textbraceleft  \textdollar, ) , + \textbraceright$

$FOLLOW (Т') = \textbraceleft  \textdollar, ) , + \textbraceright$

$FOLLOW (F) = \textbraceleft  \textdollar, ) , +, * \textbraceright$ 



## Практическая часть

В методе рекурсивного спуска полностью сохраняются идеи нисходящего разбора, принятые в `LL(1)`-грамматиках:
- происходит последовательный просмотр входной строки слева-направо;
- очередной символ входной строки является основанием для выбора одной из правых частей правил группы при замене текущего нетерминала;
- терминальные символы входной строки и правой части правила "Взаимно уничтожаются";
- обнаружение нетерминала в правой части рекурсивно повторяет этот же процесс.

В методе рекурсивного спуска эти идеи претерпевают следующие изменения:
- каждому нетерминалу соответствует отдельная процедура (функция), распознающая (выбирающая и «закрывающая») ОднУ из правых частей правила, имеющего в левой части этот нетерминал (т.е. для каждой группы правил пишется свой распознаватель);
- 	во входной строке имеется указатель (индекс) на текущий «закрываемый символ». Этот символ и является основанием для выбора необходимой правой части правила. Сам выбор «Зашит» в распознавателе в виде конструкций `if`  или `switch`. Правила выбора базируются на построении множеств выбирающих символах, как это принято в `LL(1)`-грамматике;
- просмотр  выбранной части  реализован в  тексте процедУРы-распознавателя путем   сравнения ожидаемого символа правой части и текущего символа входной строки;
- если в правой части ожидается терминальный символ, и он совпадает с очередным символом входной строки,  то  символ  во входной  строке  пропускается,  а распознаватель  переходит  к  следующему символу правой части;
- несовпадение  терминального символа   правой   части   и   очередного символа входной   строки свидетельствует о синтаксической ошибке;
- если  в  правой  части  встречается  нетерминалъный   символ,  то  для  него  необходимо  вызвать аналогичную распознающую процедуру (функцию).

Использование рекурсивного спуска позволяет достаточно быстро и наглядно писать программу распознавателя на основе имеющейся грамматики. Главное, чтобы последняя соответствовала требуемому ВидУ. Естественно, возникает вопрос: если грамматика не является `LL(1)`, то существует ли эквивалентная КС­ грамматика, для которой метод рекурсивного спуска применим? К сожалению, нет алгоритма, отвечающего на посrавленный вопрос, т.е. это алгоритмически неразрешимая проблема.

Чтобы применить метод рекурсивного спуска, необходимо преобразовать грамматику к виду, в котором множества FIRST не пересекаются. Этот процесс может оказаться сложным. Поэтому на практике часто используется прием, называемый рекурсивным спуском с возвратами. Для этого лексический анализатор представляется в виде объекта, у которого помимо традиционных методов  `scan`,   `next` и т. п., есть также копирующий конструктор. Затем во всех ситуациях, где может возникнуть неоднозначность, перед началом разбора запоминается текущее состояние лексического анализатора (т.е. заводится копия лексического анализатора) и делается попытка продолжить разбор текста, считая, что рассматривается первая из возможных в данной ситуации конструкций. Если этот вариант разбора заканчивается неудачей, то восстанавливается состояние лексического анализатора и делается попытка заново разобрать тот же самый фрагмент с помощью следующего варианта грамматики и т. д. Если все варианты разбора заканчиваются неудачно, то вызывается функция обработки и  нейтрализации ошибки. Такой метод разбора потенциально медленнее, чем рекурсивный спуск без возвратов, но в этом случае удается сохранить грамматику в ее оригинальном виде и сэкономить усилия программиста.

Именно таким образом реализован синтаксический разбор в демонстрационном компиляторе С-бемоль [2].

**Пример**. Рассматривается текст незавершенной программы распознавания вложенности скобок, использующей метод рекурсивного спуска.

```cpp
// Рекурсивный распознаватель вложенности круглых скобок.
// Построен на основе правил следующей q-грамматики:
// 1. S -> (В)В
// 2. В -> (В)В
// 3. B -> empty

#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <stack>
using namespace std;
striпg str; // Строка с входной цепочкой, имитирующая входную ленту
iпt i; // Текущее положение входной головки
int erFlag; // Флаг, фиксирующий наличие ошибок в середине правила 

// Функция, реализующая чтение символов в буфер из входного потока. 
// Используется для ввода с клавиатуры распознаваемой строки.
// Ввод осуществляется до нажатия на клавишу Eпter.
// Символ '\n' яляется концевым маркером входной строки. 
void GetOneLine (istream &is, striпg &str) {
    char ch;
    str = "";
    for(;;) {
        is.get(ch);
        if(is.fail() || ch == '\n' ) break;
        str += ch;
    }
    str += '\n'; // Добавляется концевой маркер
}

// Функция, реализующая распознавание нетерминала В. 
bool В() {
    if(str[i]== '('){
        i++;
        if (B() )  {
            if (str[i]== ')'){ i++;
                if(B()) { // Если последний нетерминал корректен,
                          // то корректно и все правило. 
                    return true;
                } else {  // Ошибка в нетерминале в
                    erFlag++;
                    cout << "Position " << i << ", " << "Error 1: Incorrect last В !\п";
                    return false;
                }
            } else { // Это не ')' erFlag++;
                cout << "Position " << i << ", " << "Error 2: I want \')\'!\n";
                returп false;
            }
        } else {
            erFlag++;
            cout << "Position " << i << ", "
                 << "Error З: Incorrect first B !\n"; 
            return false;
        }
    }else  // Смотрим другую альтернативу: 
        return true; // пустая цепочка допустима
}


// Функция, реализующая распознавание нетерминала S.
bool S(){
    if(str[i]== '('){
        i++;
        if(B()){
            if (str[i]== ')') { 
                i++;
                if(B()) {
                    if(str [i]== '\n'){
                        return tгue; // за последним нетерминалом - конец
                    } else { // там ничего не должно быть
                        erFlag++;
                        cout << "Position " << i << ", "
                             << "Error 4: I want string end after last right bracket!\n";
                        return false;
                    }
                } else { 
                    erFlag++;
                    cout << "Position " << i << ", "
                         << "Error 4: Incorrect last B !\n";
                    return false;
                }
            } else {
                erFlag++;
                cout << "Position " << i << ", "
                     << "Error 5: I want \')\'! \п";
                return false;
            }
        } else {
            erFlag++;
            cout << "Position " << i << ", "
                 << "Error 6: Incorrect first В !\п";
            return false;
        }
    }
    return false; // первый символ цепочки некорректен
                  // то, что это ошибка, лучше определить снаружи
}

// Функция запускающая разбор и определяющая корректность его завершения,
// если первый символ не принадлежит цепочке 
bool Parser (){
    // Начальная инициализация.
    erFlag = 0;
    i= 0;
    // Процесс пошел! 
    if(S())
        return true; // Все прошло нормально 
    else {
        if(erFlag)
            cout << "Position " << i << " , "
                 << "Error 7: Internal Error !\n";
        else
            cout << "Position " << i << ", "
                 << "Error 8: Incorrect first symbol of S! \n";
        return false; // Есть ошибки
    }
}


int _tmain(int argc, _TCHAR* argv []) {
    // ...
    return 0;
}
```


## Варианты грамматик

### Вариант 1. Грамматика $G_1$
Рассматривается грамматика выражений отношения с правилами

```
<выражение> ->
    <простое выражение> |
    <простое выражение> <операция отношения> <простое выражение>

<простое выражение> ->
    <терм> |
    <знак> <терм> |
    <простое выражение> <операция типа сложения> <терм>

<терм> ->
    <фактор> |
    <терм> <операция типа умножения> <фактор>

<фактор> ->
    <идентификатор>   1
    <константа> |
    ( <простое выражение> ) |
    поt <фактор>

<операция отношения> ->
    = | <> | < | <= | > | >=

<знак> ->
    + | -

<операция типа сложения> ->
    + | - | or

<операция типа умножения> ->
    * | / | div | mod | and
```

Замечания.
1.	Нетерминалы `<идентификатор>` и `<константа>` - это лексические единицы (лексемы), которые оставлены неопределенными, а при выполнении лабораторной работы можно либо рассматривать их как терминальные символы, либо определить их по своему усмотрению и добавить эти определения.
2. Терминалы `not`, `or`, `div`, `mod`, `and` - ключевые слова (зарезервированные).
3. Терминалы `(` `)` - это разделители и символы пунктуации.
4.	Терминалы `=` `<>` `<` `<=` `>` `>=` `+` `-` `*` `/` - это знаки операций.
5.	Нетерминал `<выражение>` - это начальный символ грамматики.

### Вариант 2. Грамматика $G_2$

Рассматривается грамматика выражений отношения с правилами

```
<выражение> ->
    <арифметическое выражение> <операция отношения> <арифметическое выражение> |
    <арифметическое выражение>

<арифметическое выражение> ->
    <арифметическое выражение> <операция типа сложения> <терм> |
    <терм>

<терм> ->
    <терм> <операция типа умножения> <фактор> |
    <фактор>

<фактор> ->
    <идентификатор> |
    <константа> |
    ( <арифметическое выражение> )

<операция отношения> ->
    < | <= | = | <> | > | >=

<операция типа сложения> ->
    + | -

<операция типа умножения> ->
    * | /
```

Замечания.
1. Нетерминалы `<идентификатор>` и `<константа>` - это лексические единицы (лексемы), которые оставлены неопределенными, а при выполнении лабораторной работы можно либо рассматривать их как терминальные символы, либо определить их по своему усмотрению и добавить эти определения.
2.	Терминалы `(` `)` - это разделители и символы пунктуации.
3.	Терминалы `<` `<=` `=` `<>` `>` `>=` `+` `-` `*` `/` - это знаки операций.
4.	Нетерминал `<выражение>` - это начальный символ грамматики.

### Вариант 3. Грамматика $G_З$

Рассматривается грамматика выражений отношения с правилами

```
<выражение> ->
    <арифметическое выражение> <знак операции отношения> <арифметическое выражение>

<арифметическое выражение> ->
    <терм> |
    <знак операции типа сложения> <терм> |
    <арифметическое выражение> <знак операции типа сложения> <терм>

<терм> ->
    <множитель> |
    <терм> <знак операции типа умножения> <множитель>

<множитель> ->
    <первичное выражение> |
    <множитель> ^ <первичное выражение>

<первичное выражение> ->
    <число> |
    <идентификатор>   |
    ( <арифметическое выражение> )

<знак операции типа сложения> ->
    + | -

<знак операции типа умножения> ->
    * | / | %

<знак операции отношения> ->
    < | <= | = | >= | > | <>
```

Замечания.

1. Нетерминалы `<идентификатор>` и `<число>` - это лексические единицы (лексемы), которые оставлены неопределенными, а при выполнении лабораторной работы можно либо рассматривать их как терминальные символы, либо определить их по своему усмотрению и добавить эти определения.
2.	Терминалы `(` `)` - это разделители и символы пунктуации.
3.	Терминалы `+` `-` `*` `/` `%` `<` `<=` `=` `>=` `>` `<>` - это знаки операций.
4.  Нетерминал `<выражение>` - это начальный символ грамматики.

### Вариант 4. Грамматика $G_4$

Рассматривается грамматика логических выражений с правилами

```
<выражение> ->
    <логическое  выражение>

<логическое выражение> ->
    <логический  одночлен> |
    <логическое выражение> ! <логический одночлен>

<логический одночлен> ->
    <вторичное логическое выражение> |
    <логический одночлен> & <вторичное логическое выражение>

<вторичное логическое выражение> ->
    <первичное логическое выражение> |
    ~ <первичное логическое выражение>

<первичное логическое выражение> ->
    <логическое значение> |
    <идентификатор>

<логическое значение> ->
    true | false

<знак логической операции> ->
    ~ | & | !
```

Замечания.
1. Нетерминал  `<идентификатор>` это лексическая единица (лексемы), которая оставлена неопределенной, а при выполнении лабораторной работы можно либо рассматривать ее как терминальный символ, либо определить ее по своему усмотрению и добавить это	определение.
2. Терминалы `true`, `false` - ключевые слова (зарезервированные).
4. Терминалы `~`  `&`  `!` - это знаки операций.
5. Нетерминал `<выражение>` - это начальный символ грамматики.

### Вариант 5. Грамматика $G_5$

Рассматривается грамматика выражений с правилами

```
<выражение> ->
    <отношение> {<логическая операция> <отношение> }

<отношение> ->
    <простое выражение> [ <операция отношения> <простое выражение> ]

<простое выражение> ->
    [ <унарная аддитивная операция> ] <слагаемое>  {<бинарная аддитивная операция> <слагаемое> }

<слагаемое> ->
    <множитель> {<мультипликативная операция> <множитель> }

<множитель> ->
    <первичное> {** <первичное> } |
    abs <первичное> |
    not <первичное>

<первичное> ->
    <числовой литерал> |
    <имя>  |
    ( <выражение> )

<логическая операция> ->
    and | or | xor

<операция отношения> ->
    < | <= | = | /> | > | >=

<бинарная аддитивная операция> ->
    + | - | &

<унарная аддитивная операция> ->
    + | -
<мультипликативная операция> ->
    * | ! | mod | rem

<операции высшего приоритета> ->
    ** |  abs |  not
```

Замечания.
1. Нетерминалы `<имя>` и `<числовой литерал>` - это лексические единицы (лексемы), которые оставлены неопределенными, а при выполнении лабораторной работы можно либо рассматривать их как терминальные символы, либо определить их по своему усмотрению и добавить эти определения.
2.	Терминалы `(` `)` - это разделители и символы пунктуации.
3.	Терминалы `<` `<=` `=` `/>` `>` `>=` `+` `-` `*` `/` `&` `**` - это знаки операций.
4.	Терминалы `and`, `or`, `xor`, `mod`, `rem`, `abs`, `not` -это знаки операций (зарезервированные).
5.	Нетерминал `<выражение>` - это начальный символ грамматики.


## Задание на лабораторную работу

Дополнить грамматику блоком, состоящим из последовательности операторов присваивания. Для реализации предлагаются два варианта расширенной грамматики.

Вариант в стиле Алгол-Паскаль.
```
<программа> ->
    <блок>

<блок> ->
    begin <список операторов> end

<список операторов>
    <оператор> | <список операторов> ; <оператор>

<оператор> ->
    <идентификатор> = <выражение>
```

Вариант в стиле Си.

```
<программа> ->
    <блок>

<блок> ->
    { <список операторов> }

<список  операторов> ->
    <оператор> <хвост>

<хвост> ->
    ; <оператор> <хвост>  |  \eps
```

Первый вариант содержит левую рекурсию, которая должна быть устранена. Второй вариант не содержит левую рекурсию, но имеет $\varepsilon$-правило. В обоих вариантах точка с запятой (`;`) ставится междУ операторами. Теперь начальным символом грамматики становится нетерминал `<программа>`. Оба варианта содержат цепное правило `<программа> -> <блок>`. Можно начальным символом грамматики назначить нетерминал `<блок>`. А можно `<блок>` считать оператором, т. е.

```
<оператор> ->
    <идентификатор>  = <выражение>  |
    <блок>
```

В последнем случае возможна конструкция с вложенными блоками. Если между символом присваивания (`=`) и символом операции отношения (`=`) возникает конфликт, то можно для любого из них ввести новое изображение, например, `:=`,`<-`, `==` и т. п.

Для   модифицированной грамматики написать программу нисходящего синтаксического анализа с использованием метода рекурсивного спуска.


## Рекомендуемая  литература

1.	АХО А.В, ЛАМ М.С., СЕТИ Р., УЛЬМАН Дж.Д. Компиляторы: принципы, технологии инструменты. - М.:Вильямс, 2008.
2.	Разработка компиляторов на платформе .NET / А. Терехов, Н. Вояковская, Д. Булычев, А. Москаль.
3. D. Grune, С. Н. J. Jacobs "Parsing Techniques - А Practical Guide", Ellis Horwood, 1990. URL:   http://www.cs.vu.nl/~dick/PTAPG.html
4.	Introduction to Recursive Descent Parsing. URL: http://ag-kastens.uni-paderbom.de/lehre/material/uebi/parsdemo/recintro.html
5.	Parsing Expressions Ьу Recursive Descent. URL: http://www.engr.mun.ca/~theo/Мisc/exp_parsing.htm
6.	Recursive descent parsing - add expressions. URL: http://pages.cpcs.ucalgary.ca/~robin/class/411/intro.3.html
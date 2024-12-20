# АНАЛИЗ ДАННЫХ В РАЗРАБОТКЕ ИГР
Отчет по лабораторной работе #5 выполнила:
- Миногина Дарья Алексеевна
- РИ-230932
  
Отметка о выполнении заданий:

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Найдите внутри C# скрипта “коэффициент корреляции” и сделать выводы о том, как он влияет на обучение модели
- Задание 2.
- Изменить параметры файла yaml-агента и определить какие параметры и как влияют на обучение модели. Привести описание не менее трех параметров
- Задание 3.
- Приведите примеры, для каких игровых задачи и ситуаций могут использоваться примеры 1 и 2 с ML-Agent’ом. В каких случаях проще использовать ML-агент, а не писать программную реализацию решения?
- Выводы.
- ✨Magic✨

## Цель работы
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.

## Вводная ч
###  

## Задание 1
###  Найдите внутри C# скрипта “коэффициент корреляции” и сделать выводы о том, как он влияет на обучение модели

Выводы о влиянии параметров и логики кода на обучение модели
Параметры, передаваемые в нейронную сеть:

speedMove: Эта величина определяет скорость передвижения агента. Скорость влияет на время достижения объектов (шахты или деревни), что косвенно влияет на количество выполненных операций за эпизод. Слишком высокая или низкая скорость может привести к неоптимальному использованию времени.
timeMining: Время добычи золота напрямую связано с задержкой в процессе выполнения задачи. Оно добавляет временные ограничения, влияя на эффективность и стратегию агента.
amountGold: Количество добываемого золота контролирует доходы агента за цикл. Это ключевой параметр, от которого зависит успешность выполнения задачи.
pickaxeCost: Стоимость кирки влияет на конечную себестоимость добычи золота, что отражается в вычислениях инфляции. Неоптимальное значение увеличивает расходы, ухудшая результат обучения.
profitPercentage: Процент прибыли на добычу золота регулирует баланс между доходами и затратами. Он участвует в расчетах экономических показателей, таких как инфляция.
Корреляция между параметрами и инфляцией (tempInf):

Финальной целью агента является поддержание инфляции на уровне ниже 6%. Для этого агент должен находить баланс между значениями pickaxeCost, amountGold и profitPercentage.
Чем точнее агент оптимизирует эти параметры, тем выше вероятность достижения положительного вознаграждения (SetReward(1.0f)).
Логика поведения агента:

Агенты активно перемещаются между деревней и шахтой, выполняя цикл добычи золота и возвращения. Важно, чтобы действия агента, такие как скорость передвижения и время добычи, соответствовали оптимальным условиям для минимизации затрат.
Использование корутин (StartGoldMine, StartMonth) добавляет временные ограничения, которые обеспечивают реалистичность симуляции и необходимость временного планирования у агента.
Цикличность и обучение:

Каждые два месяца агент пересчитывает инфляцию, что становится основным критерием для завершения эпизода. Ошибки в управлении параметрами приведут к отрицательному вознаграждению.
Такая постановка задачи требует от агента не только выполнения базовых действий, но и стратегического управления ресурсами.
Выводы о влиянии на обучение:

Чем точнее агент сможет "научиться" устанавливать параметры speedMove, timeMining, amountGold, pickaxeCost и profitPercentage, тем выше его награда.
Агент проходит процесс обучения, стремясь минимизировать инфляцию и повышать свою прибыль. Это позволяет ему эффективно использовать стратегии экономического управления.
Основным фактором успеха модели является способность агента находить баланс между затратами и прибылью, что моделируется через множество итераций с учетом обратной связи (вознаграждения).
Таким образом, модель на основе взаимодействия с окружением обучается оптимизации экономических и временных параметров. Этот подход позволяет агенту принимать решения, основанные на наблюдениях и прошлых опытах, а также развивает навыки стратегического планирования.

## Задание 2
### Построить графики зависимости количества эпох от ошибки  обучения. Указать от чего зависит необходимое количество эпох обучения.

У меня получились следующие результаты: https://docs.google.com/spreadsheets/d/16u5VYNN4rtL8IFkjwBF_Q1-vaZFuLSgz3QGfbrxCafE/edit?usp=sharing
![GoogleSheetsPerceptron](https://github.com/MidoriKsai/Homework4/blob/main/GoggleSheetsPerceptron.png)

#### Необходимое количество эпох обучения зависит от сложности операции и её линейной разделимости. Простые операции, такие как OR, AND и NAND, являются линейно разделимыми, поэтому перцептрон справляется с ними достаточно быстро, требуя меньше эпох. В отличие от них, операция XOR не является линейно разделимой, и для её реализации стандартный перцептрон не подходит.

## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity.

Я выбрала способ визуализации - столкновение с объектом. Создала на сцене шар и платформу, при запуске и шару, и платформе задаются параметры, потом из текста берется значение и сравнивается с ранее введенными. Если получилось 0 - при столкновении платформа окрашивается в красный, а при 1 - в зеленый
![Ball](https://github.com/MidoriKsai/Homework4/blob/main/Ball.png)
![Platform](https://github.com/MidoriKsai/Homework4/blob/main/Platform.png)
![BallText](https://github.com/MidoriKsai/Homework4/blob/main/BallText.png)
![Text](https://github.com/MidoriKsai/Homework4/blob/main/Text.png)

Результаты для OR, в качестве примера:

![FailAndSucsess](https://github.com/MidoriKsai/Homework4/blob/main/FailAndSucsess.png)
![Fail](https://github.com/MidoriKsai/Homework4/blob/main/Fail.png)
![Suscess](https://github.com/MidoriKsai/Homework4/blob/main/Suscess.png)

Код с Перцептроном который я накидывала на платформу 
```csharp
using System.Collections;
using System.Threading.Tasks;
using TMPro;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
    public double[] input;
    public double output;
}

public class ChangeColors : MonoBehaviour
{
    public GameObject textMeshPro;
    public TrainingSet[] ts; 
    private double[] weights = { 0, 0 }; 
    private double bias = 0;
    private double totalError = 0;

    public GameObject platform;
    public Color successColor = Color.white;
    public Color failureColor = Color.black;

    double DotProductBias(double[] v1, double[] v2)
    {
        double d = 0;
        for (int x = 0; x < v1.Length; x++)
        {
            d += v1[x] * v2[x];
        }
        d += bias;
        return d;
    }

    double CalcOutput(double[] input)
    {
        double dp = DotProductBias(weights, input);
        return dp > 0 ? 1 : 0;
    }

    void InitialiseWeights()
    {
        for (int i = 0; i < weights.Length; i++)
        {
            weights[i] = Random.Range(-1.0f, 1.0f);
        }
        bias = Random.Range(-1.0f, 1.0f);
    }

    void UpdateWeights(int j)
    {
        double error = ts[j].output - CalcOutput(ts[j].input);
        totalError += Mathf.Abs((float)error);
        for (int i = 0; i < weights.Length; i++)
        {
            weights[i] = weights[i] + error * ts[j].input[i];
        }
        bias += error;
    }
    void Train(int epochs)
    {
        InitialiseWeights();
        for (int e = 0; e < epochs; e++)
        {
            totalError = 0;
            for (int t = 0; t < ts.Length; t++)
            {
                UpdateWeights(t);
            }
            Debug.Log("Epoch " + e + " Total Error: " + totalError);
        }
    }

    void Start()
    {
        Train(1);
    }

    void OnCollisionEnter(Collision other)
    {
        var text = textMeshPro.GetComponent<TextMeshProUGUI>().text;
        int thisNumber = int.Parse(text[text.Length - 1].ToString());
        var otherNumber = other.gameObject.GetComponent<CollisionBall>().Int;
        foreach (var str in ts)
        {
            if (str.input[0] == thisNumber && str.input[1] == otherNumber)
            {
                if (str.output == 1)
                {
                    gameObject.GetComponent<Renderer>().material.color = successColor;
                }
                else
                {
                    gameObject.GetComponent<Renderer>().material.color = failureColor;
                }
            }
        }

    }
}

```

Код, который использовала для шара
```csharp
using System.Collections;
using System.Collections.Generic;
using TMPro;
using UnityEngine;

public class CollisionBall : MonoBehaviour
{
    public int Int = 11;
    public GameObject text;
    void Start()
    {
        var temp = text.GetComponent<TextMeshProUGUI>().text;
        Int = int.Parse(temp[temp.Length - 1].ToString());
    }

}


```

## Выводы

Лабораторная работа продемонстрировала базовые принципы работы перцептрона и его способность обучаться на линейно разделимых данных. Для более сложных задач (например, XOR) требуется использование другого подхода. Практическая реализация и визуализация в Unity укрепили понимание основ машинного обучения и работы перцептрона.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

# LR-17[LR-17.zip](https://github.com/Maxsim2418/LR-17/files/10146048/LR-17.zip)

Борнеман М.А

ЭВТ-70

Игровой движок:Unity

Лабораторная работа №17

Тема: разработка игрового проекта Puzzle

Цель: приобрести навыки в разработке игрового проекта Puzzle

Ход работы:

1.	Выполнение работы

1.	Был настроен задний фон и были размещены игровые объекты

![image](https://user-images.githubusercontent.com/119674602/205433168-58a66aa1-8169-4856-a7f0-ad3e76a6cb43.png)

Рисунок 17.1 – Постройка сцены.

2.	Был написан скрипт для проверки и подсчёта объектов с правильным положением. Если все объекты имеют правильное положение, то игрок победил.

Листинг 17.1 GameManager.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameManager : MonoBehaviour
{
    public GameObject PipesHolder;
    public GameObject[] Pipes;

    [SerializeField]
    int totalPipes = 0;
    [SerializeField]

    int correctPipes = 0;

    void Start()
    {
        totalPipes = PipesHolder.transform.childCount;

        Pipes = new GameObject[totalPipes];

        for (int i = 0; i < Pipes.Length; i++)
        {
            Pipes[i] = PipesHolder.transform.GetChild(i).gameObject;
        }
    }

    public void correctMove()
    {
        correctPipes += 1;

        Debug.Log("correct Move");

        if (correctPipes == totalPipes)
        {
            Debug.Log("You win!");
        }
    }

    public void wrongMove()
    {
        correctPipes -= 1;
    }

}


3.	Был написан скрипт для управления игровыми объектами по нажатию клавиши мыши.

Листинг 17.2 PipeScript.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PipeScript : MonoBehaviour
{
    float[] rotations = { 0, 90, 180, 270 };

    public float[] correctRotation;
    [SerializeField]
    bool isPlaced = false;

    int PossibleRots = 1;

    GameManager gameManager;

    private void Awake()
    {
        gameManager = GameObject.Find("GameManager").GetComponent<GameManager>();
    }

    private void Start()
    {
        PossibleRots = correctRotation.Length;
        int rand = Random.Range(0, rotations.Length);
        transform.eulerAngles = new Vector3(0, 0, rotations[rand]);

        if(PossibleRots > 1)
        {
            if (transform.eulerAngles.z == correctRotation[0] || transform.eulerAngles.z == correctRotation[1])
            {
                isPlaced = true;
                gameManager.correctMove();
            }

        }
        else
        {
            if (transform.eulerAngles.z == correctRotation[0])
            {
                isPlaced = true;
                gameManager.correctMove();
            }
        }

    }



    private void OnMouseDown()
    {
        transform.Rotate(new Vector3(0, 0, 90));

        if (PossibleRots > 1)
        {
            if (transform.eulerAngles.z == correctRotation[0] || (transform.eulerAngles.z == correctRotation[1] && isPlaced == false))
            {
                isPlaced = true;
                gameManager.correctMove();
            }
            else if (isPlaced == true)
            {
                isPlaced = false;
                gameManager.wrongMove();
            }
        }
        else
        {
            if (transform.eulerAngles.z == correctRotation[0]  && isPlaced == false)
            {
                isPlaced = true;
                gameManager.correctMove();
            }
            else if (isPlaced == true)
            {
                isPlaced = false;
                gameManager.wrongMove();
            }
        }

        
    }
}

2. Вывод.

В ходе проделанной работы был разработан игровой проект Puzzle.

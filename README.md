Program.cs
using System;
using System.Collections.Generic;
using System.IO;
using Newtonsoft.Json;
using System.Timers;

class Program
{
    static List<Question> questions;
    static int score = 0;
    static Timer timer;
    static int timeLimit = 10; 

    static void Main(string[] args)
    {
        LoadQuestions();
        StartQuiz();
    }

    static void LoadQuestions()
    {
        string json = File.ReadAllText("Question.json");
        questions = JsonConvert.DeserializeObject<List<Question>>(json);
    }

    static void StartQuiz()
    {
        foreach (var question in questions)
        {
            Console.WriteLine(question.QuestionText);
            for (int i = 0; i < question.Options.Count; i++)
            {
                Console.WriteLine($"{i + 1}. {question.Options[i]}");
            }

            timer = new Timer(timeLimit * 1000);
            timer.Elapsed += TimerElapsed;
            timer.Start();

            Console.Write("Your answer (1-4): ");
            string input = Console.ReadLine();
            timer.Stop();

            if (int.TryParse(input, out int answerIndex) && answerIndex > 0 && answerIndex <= question.Options.Count)
            {
                if (question.Options[answerIndex - 1] == question.Answer)
                {
                    score++;
                    Console.WriteLine("Correct!");
                }
                else
                {
                    Console.WriteLine($"Wrong! The correct answer is: {question.Answer}");
                }
            }
            else
            {
                Console.WriteLine("Invalid input. Moving to the next question.");
            }

            Console.WriteLine();
        }

        Console.WriteLine($"Your final score is: {score}/{questions.Count}");
    }

    static void TimerElapsed(object sender, ElapsedEventArgs e)
    {
        Console.WriteLine("\nTime's up!");
        timer.Stop();
    }
}

class Question
{
    public string QuestionText { get; set; } 
    public List<string> Options { get; set; }
    public string Answer { get; set; }
}


Question.json
[
  {
    "QuestionText": "What is the capital of France?",
    "Options": [ "Berlin", "Madrid", "Paris", "Rome" ],
    "Answer": "Paris"
  },
  {
    "QuestionText": "Which planet is known as the Red Planet?",
    "Options": [ "Earth", "Mars", "Jupiter", "Saturn" ],
    "Answer": "Mars"
  },

  {
    "QuestionText": "Which of the three banks will be merged with the other two to create India’s third-largest bank?",
    "Options": [ "Punjab National Bank ", "Indian Bank ", "Bank of Baroda", "Dena Bank" ],
    "Answer": "Indian Bank"
  },

  {
    "QuestionText": "Where was India’s first national Museum opened?",
    "Options": [ "Delhi", "Hyderabad", "Mumbai", "rajasthan" ],
    "Answer": "Mumbai"
  },

  {
    "QuestionText": "Vijay Singh (golf player) is from which country?",
    "Options": [ "UK", "USA", "India", "Fiji" ],
    "Answer": "Fiji"
  },

  {
    "QuestionText": "What is the full form of DRDL?",
    "Options": [ "Differential Research and Documentation Laboratory", "Department of Research and Development Laboratory", "Defense Research and Development Laboratory", "None of These" ],
    "Answer": "Department of Research and Development Laboratory"
  },

  {
    "QuestionText": "What is the reason behind the Bats flying in the dark?",
    "Options": [ "They produce high pitched sounds called ultrasonics", "The light startles them", "They have a perfect vision in the dark", "None of these" ],
    "Answer": "They produce high pitched sounds called ultrasonics"
  },

  {
    "QuestionText": "The largest public sector undertaking in the country is?",
    "Options": [ "Railways", "Roadways", "Airways", "Steel and Iron Plants" ],
    "Answer": "Railways"
  },

  {
    "QuestionText": "Why is the color of papaya yellow?",
    "Options": [ "Lycopene", "Papain", "Carotene", "Caricaxanthin" ],
    "Answer": "Caricaxanthin"
  },

  {
    "QuestionText": "Which of the following gives the correct increasing order of the atomic radii of O, F and N ?",
    "Options": [ "O, N, F", "F, O, N", "N, F, O", "O, F, N" ],
    "Answer": "F, O, N"
  }


]

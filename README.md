# Zmeyca
using CsSnake;
using System;
using System.Collections.Generic;
using System.Threading;
using static System.Net.Mime.MediaTypeNames;

namespace CsSnake
{
    struct Location
    {
        public int X;
        public int Y;

        public Location(int x, int y)
        {
            this.X = x;
            this.Y = y;
        }
    };

    enum Direction { Left, Right, Up, Down };

    class Program
    {
        static Direction direction;
        static List<Location> snake = new List<Location>();
        static Location star = new Location(60, 20);

        public static void Main(string[] args)
        {
            Console.OutputEncoding = System.Text.Encoding.GetEncoding(1252);
            Console.Title = "Snake Ranch";

            Location head = new Location(40, 12);
            snake.Add(head);

            direction = Direction.Right;

            Thread thread = new Thread(Move);
            thread.IsBackground = true;
            thread.Start();

            while (true)
            {
                // handle input

                ConsoleKeyInfo keyInfo = Console.ReadKey(true);

                switch (keyInfo.Key)
                {
                    case ConsoleKey.Escape:
                        return;
                    case ConsoleKey.UpArrow:
                        direction = Direction.Up;
                        break;
                    case ConsoleKey.DownArrow:
                        direction = Direction.Down;
                        break;
                    case ConsoleKey.LeftArrow:
                        direction = Direction.Left;
                        break;
                    case ConsoleKey.RightArrow:
                        direction = Direction.Right;
                        break;
                }
            }
        }

        public static void Move()
        {
            while (true)
            {
                var next = snake[0];

                switch (direction)
                {
                    case Direction.Left:
                        if (next.X > 0)
                            next.X--;
                        break;
                    case Direction.Right:
                        if (next.X < 79)
                            next.X++;
                        break;
                    case Direction.Up:
                        if (next.Y > 0)
                            next.Y--;
                        break;
                    case Direction.Down:
                        if (next.Y < 24)
                            next.Y++;
                        break;
                }

                Console.Clear();

                // show snake

                foreach (Location location in snake)
                {
                    Console.SetCursorPosition(location.X, location.Y);
                    Console.ForegroundColor = ConsoleColor.White;
                    Console.Write((char)178);
                    Console.ResetColor();
                }

                // show star

                Console.SetCursorPosition(star.X, star.Y);
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.Write('*');
                Console.ResetColor();

                snake.Insert(0, next);
                if (next.X == star.X && next.Y == star.Y)
                {
                    Random random = new Random();
                    star.X = random.Next(0, 80);
                    star.Y = random.Next(0, 25);
                }
                else
                    snake.RemoveAt(snake.Count - 1);

                Thread.Sleep(300);
            }
        }
    }
}


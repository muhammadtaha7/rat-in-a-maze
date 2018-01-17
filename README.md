# rat-in-a-maze
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IO;

namespace ConsoleApplication100
{
    class Rat_In_A_Maze
    {
        public void Maze(int[,] fixed_path, int[,] rat_path,int N)
        {
                bool check = true;
                for (int i = 0; i < N; i++)
                {
                    for (int j = 0; j < N; j++)
                    {
                        if ((i == N - 1) && (j == N - 1))
                        {
                            break;
                        }
                        if ((j + 1 != N) && (fixed_path[i, j + 1] == 1))               //Check for Right
                        {
                          if (rat_path[i, j + 1] == 1)
                        {
                            rat_path[i, j] = 0;
                            fixed_path[i, j] = 0;
                        }
                        rat_path[i, j + 1] = 1;
                        }
                        else if ((i + 1 != N) && (fixed_path[i + 1, j] == 1))          //Check for Down
                        {
                            if (rat_path[i + 1, j] == 1)
                            {
                                rat_path[i, j] = 0;
                                fixed_path[i, j] = 0;
                            }
                            rat_path[i + 1, j] = 1;
                            i++;
                            j--;
                        }
                        else if ((i - 1 != -1) && (fixed_path[i - 1, j] == 1))              //Check for Up
                        {
                            if (rat_path[i - 1, j] == 1)
                            {
                                rat_path[i, j] = 0;
                                fixed_path[i, j] = 0;
                            }
                            rat_path[i - 1, j] = 1;
                            i--;
                            j--;

                        }
                        else if ((j - 1 != -1) && (fixed_path[i, j - 1] == 1))              //Check for Left
                        {
                            if (rat_path[i, j - 1] == 1)
                            {
                                rat_path[i, j] = 0;
                                fixed_path[i, j] = 0;
                            }
                            rat_path[i, j - 1] = 1;
                            j = j - 2;
                        }
                        else
                        {
                            check = false;
                            break;
                        }
                    }
                   break;
                }
            if (check==false)
            {
                Console.WriteLine("Sorry! Path not found.");
            }
            else
            {

                Console.WriteLine("Path covered by Rat:");
                for (int i = 0; i < N; i++)
                {
                    for (int j = 0; j < N; j++)
                    {
                        Console.Write("\t" + rat_path[i, j]);
                    }
                    Console.WriteLine("\n");
                }
            }
        }
        public int[] ReadPath(int N_square)
        {
           int[] Input_path = new int[N_square];
            using (StreamReader sr = new StreamReader("D:\\" + "rat.txt"))
            {
                Console.WriteLine("Input path taken from file 'rat.txt'");
                for(int i=0;i<N_square;i++)
                {
                    Input_path[i] = Convert.ToInt32(sr.ReadLine());
                    Console.WriteLine(Input_path[i]);
                }
            }
            return Input_path;
        }
        public int[,] Conversion(int [] Input_path,int N)
        {
            int[,] fixed_path = new int[N, N];
            int k = 0;
            for (int i=0;i<N;i++)
            {
                for(int j=0;j<N;j++)
                {
                    fixed_path[i, j] = Input_path[k];
                    k++;
                }
            }
            return fixed_path;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("\t~()>\tRAT IN A MAZE \t<()~\n  ");
            Console.WriteLine("Enter the size of your maze:");
            int N = Convert.ToInt32(Console.ReadLine());
            int N_square = N * N;

            int[,] fixed_path = new int[N, N];
            int[,] rat_path = new int[N, N];
            rat_path[0, 0] = 1;
            int[] Input_path = new int[N_square];

            Rat_In_A_Maze r = new Rat_In_A_Maze();
            Input_path= r.ReadPath(N_square);
            fixed_path= r.Conversion(Input_path, N);

            Console.WriteLine("The path you have given to the rat: ");
            for (int i = 0; i < N; i++)
            {
                for (int j = 0; j < N; j++)
                {
                    Console.Write("\t" + fixed_path[i, j]);
                }
                Console.WriteLine("\n");
            }

            r.Maze(fixed_path, rat_path, N);
            Console.ReadLine();

        }
    }
}

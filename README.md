using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp46
{
    class Program
    {
        private static int V = 5;                                                                    
        static bool isSafe(int v, bool[,] graph, int[] path, int pos)

        {
            /* Check if this vertex is an adjacent vertex of the previously
               added vertex. */
            if (graph[path[pos - 1], v] == Convert.ToBoolean(0))                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
                return false;

            /* Check if the vertex has already been included.
              This step can be optimized by creating an array of size V */
            for (int i = 0; i < pos; i++)
                if (path[i] == v)
                    return false;

            return true;
        }

        static bool hamCycleUtil(bool[,] graph, int[] path, int pos)
        {
            /* base case: If all vertices are included in Hamiltonian Cycle */
            if (pos == V)
            {
                // And if there is an edge from the last included vertex to the
                // first vertex
                if (graph[path[pos - 1], path[0]] == Convert.ToBoolean(1))
                    return true;
                return false;
            }

            // Try different vertices as a next candidate in Hamiltonian Cycle.
            // We don't try for 0 as we included 0 as starting point in in hamCycle()
            for (int v = 1; v < V; v++)
            {
                /* Check if this vertex can be added to Hamiltonian Cycle */
                if (isSafe(v, graph, path, pos))
                {
                    path[pos] = v;

                    /* recur to construct rest of the path */
                    if (hamCycleUtil(graph, path, pos + 1) == true)
                        return true;

                    /* If adding vertex v doesn't lead to a solution,
                       then remove it */
                    path[pos] = -1;
                }
            }

            /* If no vertex can be added to Hamiltonian Cycle constructed so far,
               then return false */
            return false;
        }


        static void hamCycle(bool[,] graph)
        {
            var path = new int[V];
            for (int i = 0; i < V; i++)
                path[i] = -1;

            /* Let us put vertex 0 as the first vertex in the path. If there is
               a Hamiltonian Cycle, then the path can be started from any point
               of the cycle as the graph is undirected */
            path[0] = 0;
            if (hamCycleUtil(graph, path, 1) == false)
            {

                Console.WriteLine("\nSolution does not exist because its not hamiltonian cycle");
                Console.ReadLine();
                return;
            }
            printSolution(path);
            return;
        }

        static void printSolution(int[] path)
        {

            Console.Write(@"Solution Exists:" +

                          " Following is one Hamiltonian Cycle \n");
            for (int i = 0; i < V; i++)
                Console.WriteLine(path[i]);

            // Let us print the first vertex again to show the complete cycle
            Console.WriteLine(path[0]);
            Console.WriteLine("\n");
        }

        static void Main(string[] args)
        {
            Console.WriteLine("Graph 1 :-\n");
            Console.WriteLine(@"
      (0)--(1)--(2)
       |   / \   |
       |  /   \  |
       | /     \ |
      (3)-------(4)");
            bool[,] graph1 ={{false, true, false, true, false},
                     {true, false, true, true, true},
                      {false, true, false, false, true},
                      {true, true, false, false, true},
                     {false, true, true, true, false},
                    };

            // Print the solution
            hamCycle(graph1);
            Console.WriteLine("Graph 2 :-\n");
            Console.WriteLine(@"
      (0)--(1)--(2)
       |   / \   |
       |  /   \  |
       | /     \ |
      (3)       (4)");
            bool[,] graph2 = {{false, true, false, true, false},
                      {true, false, true, true, true},
                      {false, true, false, false, true},
                      {true, true, false, false, false},
                      {false, true, true, false, false},
                     };

            // Print the solution
            hamCycle(graph2);
        }
    }
}



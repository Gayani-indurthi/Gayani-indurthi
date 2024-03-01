*what is bankers algorithm?
Banker’s algorithm is an algorithm to avoid deadlock and to allocate resources to the processes safely. Banker’s algorithm
helps the operating system to successfully share the resources among all the processes. 

*Advantages

Following are the essential characteristics of the Banker's algorithm:

It contains various resources that meet the requirements of each process.
Each process should provide information to the operating system for upcoming resource requests, the number of resources, and how long the resources will be held.
It helps the operating system manage and control process requests for each type of resource in the computer system.
The algorithm has a Max resource attribute that represents indicates each process can hold the maximum number of resources in a system.

*Disadvantages

It requires a fixed number of processes, and no additional processes can be started in the system while executing the process.
The algorithm does no longer allows the processes to exchange its maximum needs while processing its tasks.
Each process has to know and state their maximum resource requirement in advance for the system.
The number of resource requests can be granted in a finite time, but the time limit for allocating the resources is one year.
When working with a banker's algorithm, it requests to know about three things:

How much each process can request for each resource in the system. It is denoted by the [MAX] request.
How much each process is currently holding each resource in a system. It is denoted by the [ALLOCATED] resource.
It represents the number of each resource currently available in the system. It is denoted by the [AVAILABLE] resource.
Following are the important data structures terms applied in the banker's algorithm as follows:

Suppose n is the number of processes, and m is the number of each type of resource used in a computer system.

Available: It is an array of length 'm' that defines each type of resource available in the system. When Available[j] = K, means that 'K' instances of Resources type R[j] are available in the system.
Max: It is a [n x m] matrix that indicates each process P[i] can store the maximum number of resources R[j] (each type) in a system.
Allocation: It is a matrix of m x n orders that indicates the type of resources currently allocated to each process in the system. When Allocation [i, j] = K, it means that process P[i] is currently allocated K instances of Resources type R[j] in the system.
Need: It is an M x N matrix sequence representing the number of remaining resources for each process. When the Need[i] [j] = k, then process P[i] may require K more instances of resources type Rj to complete the assigned work.
Nedd[i][j] = Max[i][j] - Allocation[i][j].
Finish: It is the vector of the order m. It includes a Boolean value (true/false) indicating whether the process has been allocated to the requested resources, and all resources have been released after finishing its task.
The Banker's Algorithm is the combination of the safety algorithm and the resource request algorithm to control the processes and avoid deadlock in a system:
// Banker's Algorithm

#include <iostream>
using namespace std;
 
int main()
{
    // P0, P1, P2, P3, P4 are the Process names here
 
  int n, m, i, j, k;
  n = 5; // Number of processes
  m = 3; // Number of resources
  int alloc[5][3] = { { 0, 1, 0 }, // P0 // Allocation Matrix
                     { 2, 0, 0 }, // P1
                     { 3, 0, 2 }, // P2
                     { 2, 1, 1 }, // P3
                     { 0, 0, 2 } }; // P4
 
  int max[5][3] = { { 7, 5, 3 }, // P0 // MAX Matrix
                   { 3, 2, 2 }, // P1
                   { 9, 0, 2 }, // P2
                   { 2, 2, 2 }, // P3
                   { 4, 3, 3 } }; // P4
 
  int avail[3] = { 3, 3, 2 }; // Available Resources
 
  int f[n], ans[n], ind = 0;
  for (k = 0; k < n; k++) {
    f[k] = 0;
  }
  int need[n][m];
  for (i = 0; i < n; i++) {
    for (j = 0; j < m; j++)
      need[i][j] = max[i][j] - alloc[i][j];
  }
  int y = 0;
  for (k = 0; k < 5; k++) {
    for (i = 0; i < n; i++) {
      if (f[i] == 0) {
 
        int flag = 0;
        for (j = 0; j < m; j++) {
          if (need[i][j] > avail[j]){
            flag = 1;
            break;
          }
        }
 
        if (flag == 0) {
          ans[ind++] = i;
          for (y = 0; y < m; y++)
            avail[y] += alloc[i][y];
          f[i] = 1;
        }
      }
    }
  }
   
  int flag = 1;
   
  // To check if sequence is safe or not
  for(int i = 0;i<n;i++)
  {
        if(f[i]==0)
      {
        flag = 0;
        cout << "The given sequence is not safe";
        break;
      }
  }

  if(flag==1)
  {
    cout << "Following is the SAFE Sequence" << endl;
      for (i = 0; i < n - 1; i++)
        cout << " P" << ans[i] << " ->";
      cout << " P" << ans[n - 1] <<endl;
  }
 
    return (0);
}
 output:
 Following is the SAFE Sequence
 P1 -> P3 -> P4 -> P0 -> P2

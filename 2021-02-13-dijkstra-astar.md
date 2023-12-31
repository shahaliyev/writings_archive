---
title: Implementation of Dijkstra & A* Pathfinding Algorithms in Go
description: Shortest Path Algorithm in Golang
categories: [computer science]
tags: [golang, project, algorithms, medium]
---

![](https://cdn-images-1.medium.com/max/800/1*3QBejb5PpzDKurYQSEVzyQ.png)

Follows the implementation of **Dijkstra** and **A\*** pathfinding algorithms in Go programming language. Obviously.

The best article on the A star algorithm was unexpectedly on Wikipedia. Before reading further, I am assuming you already have some background knowledge on what both Dijkstra and A star pathfinding algorithms do (hint: they find the shortest path), otherwise, make sure to check out both the [Wikipedia page](https://en.wikipedia.org/wiki/A*_search_algorithm) and a [fine article](https://medium.com/@nicholas.w.swift/easy-a-star-pathfinding-7e6689c7f7b2) on the implementation of A star. In essence, Dijkstra by itself is A star algorithm without heuristics, which will become clear pretty soon.

## Implementation of A* and Dijkstra in Go

Again, I am assuming that you are good with the theory. If no, at least go (ahem) and check the pseudocode for the algorithm. The word “algorithm” is in the singular form, as we are going to implement only one function that will do both the work of A star and Dijkstra for us.

Given an undirected graph, with lots of starting and ending vertices, as well as the distance between them. For example, the input`0 2 500` means that there is an edge between the vertex numbers 0 and 2 with a distance of 500 units. We will store them in an `Edge` array.

Also given physical vertex locations within an NxN matrix. We will use this information as our heuristics to guide our A\* algorithm. Simply, the heuristic function will return the euclidean distance between vertices in the “physical world”, which will be our estimation.

The`Edge` type has the fields of `to` and `distance` , which we will use to store our edge graph. But first, we initialize the global variables.

```go
var edgeGraph = make([][]Edge, SIZE)  
var vertexGraph = make([]int, SIZE)

var dist = make([]float64, SIZE)  
var parent = make([]int, SIZE)

var visitCount int
```

`edgeGraph[0][i]` will return the _i_th edge coming out of the vertex 0. `vertexGraph[0]` will return the physical location of vertex 0. `dist` and `parent` slices should bother you only and only if you are here by chance.

## Finding the Shortest Path with A* and Dijkstra Algorithms

As a quick recall, `CTRL (⌘) + C` copies, and the`x` button exits the tab in which you are reading this article. But c’mon.

### A* / Dijkstra Golang implementation

```go
func findShortestPath(start, finish int, algorithm string) {
	reset()

	var fCost float64
	var gCost float64
	var hCost float64

	var currentVertex, to int
	var edge Edge

	// initializing priority queue
	queue := NewPriorityQueue()
	queue.Push(Edge{start, 0})
	dist[start] = 0

	for queue.Length() > 0 {
		visitCount++

		// getting the current edge and vertex
		edge = queue.Front().(Edge)
		queue.Pop() // dequeue
		currentVertex = edge.to

		// terminating search when reaching goal
		if currentVertex == finish { break }

		// iterating through all the vertices that we can reach from current vertex
		for i := 0; i < len(edgeGraph[currentVertex]); i++ {
			// getting the next connected vertex
			to = edgeGraph[currentVertex][i].to
			// calculating gCost
			gCost = dist[currentVertex] + edgeGraph[currentVertex][i].distance

			// relaxing the edge if needed
			if gCost < dist[to] {
				// updating distance if shorter path is found
				dist[to] = gCost
				// setting heuristic in case of A*
				if algorithm == "astar" { hCost = heuristic(to, finish) }
				// calculating fCost (hCost is zero in case of dijkstra)
				fCost = gCost + hCost
				// enqueue
				queue.Push(Edge{to, fCost})
				// memorizing the parent vertex for building path
				parent[to] = currentVertex
			}
		}
	}
}
```

`findShortestPath` accepts starting and ending vertices, as well as the algorithm of choice (`dijkstra` or `astar`). Of course, you may also write `apple`instead of dijkstra; the algorithm will act in the exact same way.

The `reset` function gives default values to arrays: `dist` gets filled with infinities, and`parent` goes negative.

We then declare our costs and initialize our data structure — **priority queue.** There is no built-in priority queue implementation in Go, so we will use the simple [priority queue library in Go](https://programmer.help/blogs/simple-priority-queue-implemented-by-golang.html) by _mtorbin._ We then immediately push the start Edge to the queue.

`visitCount` simply keeps track of the visited vertices in order to compare A star to Dijkstra. We `break` the loop whenever we reach the goal node.

Now comes the most important part.

We go through all the outgoing edges of `currentVertex`. You know, we have a thing called **f(v)**, which is equal to **g(v) + h(v)**. We may calculate our **gCost**, which will be the distance until our current vertex plus the distance from our current vertex to its child node.

```go
gCost = dist[currentVertex] + edgeGraph[currentVertex][i].distance
```

If our gCost is less than the current distance until our child node, we update our `dist[to]` (“to” is the child node, obv).

```go
dist[to] = gCost
```

## The difference between Dijkstra & A* algorithms

If you knew how to implement Dijkstra, then this is the culmination. Otherwise, it’s not. The difference between Dijkstra & A* algorithms is the addition of the **heuristic** function.

If the algorithm is defined as `astar`we apply our heuristic function, otherwise, **hCost** remains 0 (in Go, zero is the default value for variables while declaration):

```go
if algorithm == "astar" { hCost = heuristic(to, finish) }

fCost = gCost + hCost
```

**fCost** becomes **gCost** and hence turns into our usual Dijkstra in case our algorithm is specified as “apple”. The heuristic function simply calculates the **euclidean distance** between the child and goal nodes. Then we simply push the edge to the queue and update our parent array in order to be able to restore our path in the future.

The full code with the heuristic function, path building, and working test cases you will find in the [Github repository](https://github.com/shahaliyev/go-dijkstra-astar "https://github.com/shahaliyev/go-dijkstra-astar")[](https://github.com/shahaliyev/go-dijkstra-astar). Pull requests if you find a bug or some other insect.


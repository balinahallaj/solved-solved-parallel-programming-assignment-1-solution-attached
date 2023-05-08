Download Link: https://assignmentchef.com/product/solved-solved-parallel-programming-assignment-1-solution-attached
<br>
1. Introduction

Using SimMud, a massively multiplayer game simulator, implement and evaluate one of the dynamic load balancing algorithms: “Lightest” or “Spread”. A presentation of these algorithms can be found in Locality Aware Dynamic Load Management for Massively Multiplayer Games (http://www.eecg.toronto.edu/~amza/papers/f87-chen.pdf ). The source code for SimMud can be found at: http://www.eecg.toronto.edu/~amza/ece1747h/homeworks/hw1/game_benchmark_1747.tgz

2. SimMud

SimMud follows a client-server architecture. The map of the game is represented as a matrix of cells, a cell being the smallest unit of the world. There a 3 types of entities: players, game objects (resources, apples) and blocks (walls). The players can perform a series of actions: move one cell at a time, eat resources (apples) and attack another player. At any point in time a player can only see as far as a square area around him defined as his “area of interest” (CLIENT_MATRIX_SIZE). From time to time players receive quests from the server. A quest is defined as a position on the map, and lasts for a limited amount of time. If by the end of the quest a player is near the quest’s location it gets rewarded. Quests are a way of reproducing flocking behaviour inside the benchmark.

The server has a multithreaded design and each thread executes a 3-staged loop divided by barriers. In the first stage requests coming from clients get processed and the world map gets updated accordingly. This stage lasts for a fixed amount of time, the “regular_update_interval”. The second stage, which needs to be executed by only one thread, does load balancing and quest planning. Finally, in the last stage each thread sends world updates back to its clients with their area of interest. These stages are implemented in “src/server/WorldUpdateModule.cpp”

3. Load Balancing

For the purpose of load balancing and synchronization the map is divided in a grid of regions. Consequently, all the entities of the map (players, resources) are associated with a region corresponding to their position. Each region is assigned to a thread which is in charge with processing requests coming from the clients from that region as well as sending them updates. The goal of load balancing is making sure that all threads do a fairly similar amount of work such that the maximum number of clients can be hosted by the server.

The default load balancing algorithm is “static” which simply divides the map between the threads in contiguous sequences of regions.
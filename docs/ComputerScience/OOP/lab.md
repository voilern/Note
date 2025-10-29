# Lab

## Assignment 1 StudentⅠ

Write a program that asks you for 10 records of students. Each record consists of a name (w/o space) and scores for three courses (in integer, 1 to 5). Output a list as follows and calculate the average score (in `float`) of each student and each course. Output the lowest and highest score for each course.  

```
no      name    score1  score2  score3  average
1       K.Weng  5       5       5       5
2       T.Dixon 4       3       2       3
3       V.Chu   3       4       5       4
4       L.Tson  4       3       4       3.66667
5       L.Lee   3       4       3       3.33333
6       I.Young 4       2       5       3.66667
7       K.Hiro  4       2       1       2.33333
8       G.Ping  4       4       4       4
9       H.Gu    2       3       4       3
10      J.Jon   5       4       3       4
        average 3.8     3.4     3.6
        min     2       2       1
        max     5       5       5
```

**Evaluation standard**

1. The result correctness
2. C++ code quality (compact and reasonable)
3. Comments quality (clean and accurate)
4. C functions like printf and scanf are not allowed

**Required Files**: Source code

**Optional Files**: CMakeLists.txt (for cmake build)

## Assignment 2 StudentⅡ

Write a CLI program that reads scoring records of students and prints out a summary sheet in the same format as the last homework, Students I.  

The user can input an arbitrary number of students. Each student can take an arbitrary number of courses. Each record consists of the course id/name and the score the student got. Please note that a student usually won't choose every available course.  

**Evaluation standard**

1. C++ code quality (clean, compact, and reasonable)
2. Comments quality

**Required Files**

Please prepare a zip package including the following items：

1. The source code
2. A text file containing your test data

## Assignment 3 Adventure

**The Story**

Adventure is a CLI game. The player has to explore a castle, which consists of several levels and many rooms. The task of the player is to find the room where the princess is prisoned and take her to leave!

There are many types of rooms, and each type has distinct exits. Note that there is a *monster* in one of the rooms, whose exact location is unknown. But once the player meets the monster, the game is over.

The program always shows information about the room into which the player enters: the name of the room, how many exits are there, and the names of all the exits. For example, the player is in the lobby of the castle when the game starts, and the program outputs:

```
Welcome to the lobby. There are 3 exits: east, west and up.
Enter your command:
```

Then the player can input the command `go` followed by the name of the exit that he/she would like to pass through, e.g.,

```
go east
```

Consequently, the player goes into the room to the east. The program gives information about that room, like what is shown above for the lobby. This process repeats.

During this process, if the player enters a room with a monster, the program shows a message, and the game is over. Likewise, once the player enters the secret room where the princess is, the program shows the conversation between the role and the princess. After that, she is ready to leave with the player. Then the player has to find their way out. The only way to leave the castle is via the lobby.

To simplify the implementation, all the printed messages and the user input are shown in English.

**Evaluation standard**

1. C++ code quality (clean, compact, and reasonable)
2. Comments quality
3. At least three different kinds of rooms
4. At least five rooms
5. The room with the monster or princess is randomly set

**Required Files**

Please prepare a zip package including the following items：

1. The source code
2. A text input file with your test data


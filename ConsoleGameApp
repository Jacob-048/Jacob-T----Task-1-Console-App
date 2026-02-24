using System;

public class Program
{
    public static void Main()
    {
        bool running = true;
        Console.Clear();

        while (running)
        {
            Console.WriteLine("==========MENU==========");

            Console.WriteLine("Enter [1] for Rock Paper Scissors (PvC)");
            Console.WriteLine("Enter [2] for Naughts and Crosses (PvP)");
            Console.WriteLine("Enter [3] for Adventurer");
            Console.WriteLine("Enter [4] to Exit");

            Console.WriteLine("==========MENU==========");


            string input = Console.ReadLine();

            switch (input)
            {
                case "1":
                    Console.Clear();
                    RPSLogic();
                    break;
                case "2":
                    Console.Clear();
                    NCLogic();
                    break;
                case "3":
                    Console.Clear();
                    MazeLogic();
                    break;
                case "4":
                    running = false;
                    break;
                default:
                    Console.WriteLine("Please Enter a valid Input");
                    Console.Clear();
                    break;

            }
        }
    }

    public static void MazeLogic()
    {
        bool shouldContinue = true;

        //map variables
        int wallWidth = 3;
        int wallWidthMin = 1;
        int wallWidthMax = 8;
        int gridSize = 15;
        float coinChance = 0.01f;

        Random rand = new Random();
        Console.WriteLine();

        int[,] arr = CreateRoom(gridSize, wallWidth, true, 0, coinChance);

        vec2Int playerPos = new vec2Int(gridSize / 2, gridSize / 2);
        int collectedCoins = 0;

        //0 = space
        //1 = wall
        //2 = coin

        while (shouldContinue)
        {
            for (int x = 0; x < arr.GetLength(0); x++)
            {
                for (int y = 0; y < arr.GetLength(1); y++)
                {
                    Console.Write(" ");
                    if (playerPos.x == x && playerPos.y == y) { Console.ForegroundColor = ConsoleColor.Red; Console.Write("P"); }
                    else if (arr[x, y] == 0) { Console.Write(" "); }
                    else if (arr[x, y] == 1) { Console.ForegroundColor = ConsoleColor.DarkGray; Console.Write("#"); }
                    else if (arr[x, y] == 2) { Console.ForegroundColor = ConsoleColor.Yellow; Console.Write("O"); }
                }
                Console.Write("\n");
            }

            Console.WriteLine("Coins: " + collectedCoins + ",    Enter 'e' to exit,    w,a,s,d to move");

            //change to ReadKey() in visual studio (doesn't work in dotnetfiddle)
            Char input = Console.ReadKey().KeyChar;
            Console.WriteLine();

            vec2Int desiredMove = new vec2Int();

            //check which way the player wants to move
            switch (input)
            {
                case 'a':
                    desiredMove = new vec2Int(0, -1);
                    break;
                case 'd':
                    desiredMove = new vec2Int(0, 1);
                    break;
                case 'w':
                    desiredMove = new vec2Int(-1, 0);
                    break;
                case 's':
                    desiredMove = new vec2Int(1, 0);
                    break;
                case 'e':
                    desiredMove = new vec2Int(0, 0);
                    shouldContinue = false;
                    break;
                default:
                    desiredMove = new vec2Int(0, 0);
                    break;
            }

            //check if the player can move
            bool canMove = CheckPlayerMove(arr, playerPos, desiredMove.x, desiredMove.y);
            bool canSwitchRooms = false;

            //need this here otherwise the player will not be able to get coins on edges
            if(playerPos.x == 0 || playerPos.x == gridSize-1 || playerPos.y == 0 || playerPos.y == gridSize-1)
            {
                canSwitchRooms = true;
            }

            if(canMove)
            {
                playerPos = new vec2Int(playerPos.x + desiredMove.x, playerPos.y + desiredMove.y);
            }

            if (arr[playerPos.x, playerPos.y] == 2)
            {
                collectedCoins++;
                arr[playerPos.x, playerPos.y] = 0;
            }

            //check if we are at one of the borders, if so, generate a new room
            if(canSwitchRooms)
            {
                int newWallSize = rand.Next(wallWidthMin, wallWidthMax);
                if ((playerPos.x >= newWallSize && playerPos.x < gridSize - newWallSize) || (playerPos.y >= newWallSize && playerPos.y < gridSize - newWallSize))
                {
                    wallWidth = newWallSize;
                }
                //leave west
                if (playerPos.x == 0 && desiredMove.x == -1) { playerPos.x = gridSize - 2; arr = CreateRoom(gridSize, wallWidth, false, 1, coinChance); }
                //leave north
                if (playerPos.y == 0 && desiredMove.y == -1) { playerPos.y = gridSize - 2; arr = CreateRoom(gridSize, wallWidth, false, 2, coinChance); }
                //leave east
                if (playerPos.x == gridSize - 1 && desiredMove.x == 1) { playerPos.x = 1; arr = CreateRoom(gridSize, wallWidth, false, 3, coinChance); }
                //leave south
                if (playerPos.y == gridSize - 1 && desiredMove.y == 1) { playerPos.y = 1; arr = CreateRoom(gridSize, wallWidth, false, 0, coinChance); }
            }
            

            Console.Clear();
        }
    }

    //function that creates the rooms for the map
    //enter dir: 0 = North, 1 = East, 2 = South, 3 = West
    //enter direction is used to not place walls where the player will be
    public static int[,] CreateRoom(int size, int wallWidth, bool isFirst, int enterDirection, float coinChance)
    {
        int[,] map = new int[size, size];
        Random rand = new Random();

        //set the wall states
        bool northWall = (rand.Next(0, 2) == 0);
        bool eastWall = (rand.Next(0, 2) == 0);
        bool southWall = (rand.Next(0, 2) == 0);
        bool westWall = (rand.Next(0, 2) == 0);

        if(enterDirection == 0) { northWall = false; }
        if(enterDirection == 1) { eastWall = false; }
        if(enterDirection == 2) { southWall = false; }
        if(enterDirection == 3) { westWall = false; }

        if(isFirst)
        {
            northWall = false;
            eastWall = false;
            southWall = false;
            westWall = false;
        }

        for(int x = 0; x < size; x++)
        {
            for(int y = 0; y < size; y++)
            {
                //start with corners
                if(x < wallWidth && y < wallWidth) { map[x, y] = 1; continue; }
                if(x < wallWidth && y > size - wallWidth - 1) { map[x, y] = 1; continue; }
                if(x > size - wallWidth - 1 && y < wallWidth) { map[x, y] = 1; continue; }
                if(x > size - wallWidth - 1 && y > size - wallWidth - 1) { map[x, y] = 1; continue; }

                //do the walls
                if(northWall && y < wallWidth) { map[x, y] = 1; continue; }
                if(eastWall && x > size - wallWidth - 1) { map[x, y] = 1; continue; }
                if(southWall && y > size - wallWidth - 1) { map[x, y] = 1; continue; }
                if(westWall && x < wallWidth) { map[x, y] = 1; continue; }

                //do random coin check
                if(rand.NextSingle() < coinChance) { map[x, y] = 2; continue; }

                map[x, y] = 0;
            }
        }

        return map;
    }

    public static bool CheckPlayerMove(int[,] arr, vec2Int playerPos, int moveX, int moveY)
    {
        vec2Int newMove = playerPos;
        newMove.x += moveX;
        newMove.y += moveY;

        if (newMove.x < 0 || newMove.x > arr.GetLength(0)-1)
        {
            //outside play bounds (x)
            return false;
        }
        else if(newMove.y < 0 || newMove.y > arr.GetLength(1)-1)
        {
            //outside play bounds (y)
            return false; 
        }

        if (arr[newMove.x, newMove.y] == 1 || arr[newMove.x, newMove.y] > 2)
        {
            //moving into wall
            return false;
        }

        //default to true (empty avaliable space)
        return true;
    }

    public int ReadDirection()
    {
        string key = Console.ReadKey().ToString();

        switch (key)
        {
            case "a":
                return -1;
            case "d":
                return 1;
            default:
                return 0;
        }
    }

    public static void NCLogic()
    {
        bool shouldExit = false;
        bool playerOneTurn = true;

        //state:
        //-id = unused;
        //1 = p1 (X)
        //2 = p2 (O)
        NCSlot[] slots = new NCSlot[9];

        for (int i = 0; i < slots.Length; i++)
        {
            slots[i] = new NCSlot();
            slots[i].state = -i;
        }

        while (!shouldExit)
        {
            Console.Clear();

            //draws the NC grid
            DrawGrid(slots);

            Console.WriteLine((playerOneTurn) ? "Player One Turn" : "Player Two Turn");
            Console.WriteLine("Input [1-9]");

            bool isValidInput = false;
            int output = 0;
            string playerInput;
            while (!isValidInput)
            {
                playerInput = Console.ReadLine();
                isValidInput = int.TryParse(playerInput, out output);
                if (output > 9 || output < 1)
                {
                    isValidInput = false;
                }

                if (!isValidInput)
                {
                    Console.WriteLine("Please enter a number between [1-9]");
                    continue;
                }

                if (slots[output - 1].state > 0)
                {
                    Console.WriteLine("Slot has been taken, choose a different slot");
                    isValidInput = false;
                    continue;
                }
            }

            slots[output - 1].state = (playerOneTurn) ? 1 : 2;

            int winVal = CheckWin(slots, playerOneTurn);

            if(winVal != 0) { Console.Clear(); }

            if (winVal == 1)
            {
                DrawGrid(slots);
                Console.WriteLine("Player One Won!");
                shouldExit = true;
            }
            else if (winVal == 2)
            {
                DrawGrid(slots);
                Console.WriteLine("Player Two Won!");
                shouldExit = true;
            }
            else if (winVal == -1)
            {
                DrawGrid(slots);
                Console.WriteLine("Tie!");
                shouldExit = true;
            }

            playerOneTurn = !playerOneTurn;

            if (shouldExit)
            {
                Console.WriteLine();
                Console.WriteLine("Play again? [y/n]");
                string input = Console.ReadLine();

                if (input == "y" || input == "Y")
                {
                    playerOneTurn = true;

                    for (int i = 0; i < slots.Length; i++)
                    {
                        slots[i] = new NCSlot();
                        slots[i].state = -i;
                    }
                    shouldExit = false;
                }
                Console.Clear();
            }
        }
    }

    public static int CheckWin(NCSlot[] slots, bool playerOneTurn)
    {
        if (slots[0].state == slots[1].state && slots[1].state == slots[2].state) { return (playerOneTurn) ? 1 : 2; }
        if (slots[0].state == slots[4].state && slots[4].state == slots[8].state) { return (playerOneTurn) ? 1 : 2; }
        if (slots[0].state == slots[3].state && slots[3].state == slots[6].state) { return (playerOneTurn) ? 1 : 2; }
        if (slots[1].state == slots[4].state && slots[4].state == slots[7].state) { return (playerOneTurn) ? 1 : 2; }
        if (slots[2].state == slots[5].state && slots[5].state == slots[8].state) { return (playerOneTurn) ? 1 : 2; }
        if (slots[2].state == slots[4].state && slots[4].state == slots[6].state) { return (playerOneTurn) ? 1 : 2; }
        if (slots[3].state == slots[4].state && slots[4].state == slots[5].state) { return (playerOneTurn) ? 1 : 2; }
        if (slots[6].state == slots[7].state && slots[7].state == slots[8].state) { return (playerOneTurn) ? 1 : 2; }

        bool isTie = true;
        for (int i = 0; i < slots.Length; i++)
        {
            if (slots[i].state <= 0) { isTie = false; break; }
        }

        if (isTie)
        {
            return -1;
        }

        return 0;
    }

    public static void DrawGrid(NCSlot[] slots)
    {
        Console.Write("=======================");
        for (int i = 0; i < 3; i++)
        {
            Console.Write("\n||     ||     ||     ||\n||  ");
            Console.Write(GetGridVisual(slots, 0 + (3 * i)));
            Console.Write("  ||  ");
            Console.Write(GetGridVisual(slots, 1 + (3 * i)));
            Console.Write("  ||  ");
            Console.Write(GetGridVisual(slots, 2 + (3 * i)));
            Console.Write("  ||");
            Console.Write("\n||     ||     ||     ||\n=======================");
        }
        Console.Write("\n");
    }

    public static string GetGridVisual(NCSlot[] slots, int index)
    {
        if (slots[index].state == 1) { return "X"; }
        else if (slots[index].state == 2) { return "O"; }

        return (index + 1).ToString();
    }

    public static void RPSLogic()
    {   
        bool continueRPS = true;

        while (continueRPS)
        {
            Console.Clear();
            Console.WriteLine("==========RPS==========");
            //1 is rock
            //2 is paper
            //3 is scissors

            //1 <- 3; 1 -> 2
            //2 <- 1; 2 -> 3
            //3 <- 2; 3 -> 1

            bool canParseInput = false;
            int inputNumber;
            Console.WriteLine("Enter (1) for Rock");
            Console.WriteLine("Enter (2) for Paper");
            Console.WriteLine("Enter (3) for Scissors");

            do
            {
                string input = Console.ReadLine();
                canParseInput = int.TryParse(input, out inputNumber);
                if (canParseInput && (inputNumber > 3 || inputNumber < 1))
                {
                    canParseInput = false;
                }

                if (!canParseInput)
                {
                    Console.WriteLine("Please Input a valid input");
                }
            }
            while (!canParseInput);

            Random rand = new Random();
            int enemyChoice = rand.Next(1, 4);

            Console.WriteLine("");

            //displays the inputs
            if (inputNumber == 1) Console.WriteLine("You Chose Rock");
            else if (inputNumber == 2) Console.WriteLine("You Chose Paper");
            else Console.WriteLine("You Chose Scissors");

            Console.WriteLine("");

            if (enemyChoice == 1) Console.WriteLine("Your Opponent Chose Rock");
            else if (enemyChoice == 2) Console.WriteLine("Your Opponent Chose Paper");
            else Console.WriteLine("Your Opponent Chose Scissors");

            Console.WriteLine("");

            //tests the players input against the enemy's
            if (enemyChoice == inputNumber) Console.WriteLine("*-*-*-*-*-*-*-*-*-\nTie!\n*-*-*-*-*-*-*-*-*-*-");
            else if (inputNumber == 1 && enemyChoice == 2) Console.WriteLine("--------------------\nRock looses to Paper, You Loose!\n--------------------");
            else if (inputNumber == 1 && enemyChoice == 3) Console.WriteLine("********************\nRock beats Scissors, You Win!!\n********************");
            else if (inputNumber == 2 && enemyChoice == 1) Console.WriteLine("********************\nPaper beats Rock, You Win!!\n********************");
            else if (inputNumber == 2 && enemyChoice == 3) Console.WriteLine("--------------------\nPaper looses to Scissors, You Loose!\n--------------------");
            else if (inputNumber == 3 && enemyChoice == 1) Console.WriteLine("--------------------\nScissors looses to Rock, You Loose!\n--------------------");
            else { Console.WriteLine("********************\nScissors beats Paper, You Win!!\n********************"); }

            Console.WriteLine("");
            Console.WriteLine("Replay? y : n");
            string replayInput = Console.ReadLine();

            Console.WriteLine("==========RPS==========");


            if (replayInput == "n")
            {
                continueRPS = false;
                Console.WriteLine("");
                Console.WriteLine("");
                Console.WriteLine("");
            }
            else if (replayInput == "y")
            {
                Console.WriteLine("");
                Console.WriteLine("");
                Console.WriteLine("");
            }
            else
            {
                continueRPS = false;
                Console.WriteLine("Invalid Input, Defaulting to (n)");
                Console.WriteLine("");
                Console.WriteLine("");
                Console.WriteLine("");
            }
        }

        Console.Clear();
    }
}

public class NCSlot()
{
    public int state;
}

public struct Vector2
{
    public Vector2(float x, float y)
    {
        this.x = x;
        this.y = y;
    }

    public float x { get; }
    public float y { get; }

    public override string ToString() => $"({x}, {y})";

    public static Vector2 operator +(Vector2 a, Vector2 b)
    {
        return new Vector2(a.x + b.x, a.y + b.y);
    }
}

public struct vec2Int
{
    public int x { get; set; }
    public int y { get; set; }

    public vec2Int(int x, int y)
    {
        this.x = x;
        this.y = y;
    }

    public override string ToString() => $"({x}, {y})";
}

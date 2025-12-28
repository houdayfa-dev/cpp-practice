#include <iostream>
#include <string>


using namespace std;

int RandomNumberInRange(int From, int To)
{
    int RandomNumber = 0;

    RandomNumber = rand() % (To - From + 1) + From;

    return RandomNumber;

}

int ReadNumberInRange(string Message, int From, int To)
{
    int Number = 0;

    bool IsMessage = false;

    do {
        if (IsMessage) {
            cout << "Wrong Number, the number must be between ["
                << From << "-" << To << "]" << endl;
            
        }
        cout << Message;
        cin >> Number;

        IsMessage = true;

    } while (Number < From || Number > To);

    return Number;
}

enum enGameChoices { Stone = 1, Paper = 2, Scissors = 3 };

struct strRoundInfo
{
    enGameChoices PlayerChoice;
    enGameChoices ComputerChoice;
    bool IsPlayerWins = false;
    bool IsComputerWins = false;
};

struct strGameInfo
{
    short PlayerWon = 0;
    short ComputerWon = 0;
    short DrawTimes = 0;
    strRoundInfo RoundInfo;
};

void GameMenu()
{
    cout << "\nGame Menu:\n";
    cout << "1-Stone\n";
    cout << "2-Paper\n";
    cout << "3-Scissors\n";

}

enGameChoices NumberToEnum(short Number)
{
    return (enGameChoices)Number;
}

string ConvertEnumToStr(enGameChoices Choice)
{
    switch (Choice) {

    case enGameChoices::Stone:return "Stone";
    case enGameChoices::Paper:return "Paper";
    case enGameChoices::Scissors:return "Scissors";
    default:return "Unknown";
    }

}

void ScreenColor(const strGameInfo& Info)
{
    if(Info.RoundInfo.IsPlayerWins)
        system("color 2F");//Green

    else if (Info.RoundInfo.IsComputerWins)
        system("color 4F");//Red

    else
        system("color 6F");//Yellow
}

void ResetRound(strGameInfo& Info)
{
    Info.RoundInfo.ComputerChoice = enGameChoices::Stone;
    Info.RoundInfo.PlayerChoice = enGameChoices::Stone;
    Info.RoundInfo.IsComputerWins = false;
    Info.RoundInfo.IsPlayerWins = false;
}

void DecideRoundWinner(strGameInfo& Info)
{
    
    enGameChoices P = Info.RoundInfo.PlayerChoice;
    enGameChoices C = Info.RoundInfo.ComputerChoice;

    if (P == C) {
        
        ++Info.DrawTimes;
    }

    else
    {
        if (P == enGameChoices::Stone && C == enGameChoices::Scissors)
        {
            Info.RoundInfo.IsPlayerWins = true;
            
            ++Info.PlayerWon;
        }

        else if (P == enGameChoices::Stone && C == enGameChoices::Paper) {

            Info.RoundInfo.IsComputerWins = true;
            
            ++Info.ComputerWon;
        }

        else if (P == enGameChoices::Paper && C == enGameChoices::Stone) {

            Info.RoundInfo.IsPlayerWins = true;
            
            ++Info.PlayerWon;
        }

        else if (P == enGameChoices::Paper && C == enGameChoices::Scissors) {

            Info.RoundInfo.IsComputerWins = true;
            
            ++Info.ComputerWon;
        }

        else if (P == enGameChoices::Scissors && C == enGameChoices::Paper) {

                Info.RoundInfo.IsPlayerWins = true;
                
                ++Info.PlayerWon;

            }

        else if (P == enGameChoices::Scissors && C == enGameChoices::Stone)
        {
                Info.RoundInfo.IsComputerWins = true;
                
                ++Info.ComputerWon;
        }

        
    }

}

string GetRoundWinner(const strGameInfo& Info)
{
    if (Info.RoundInfo.IsPlayerWins)
        return "Player";

    else if (Info.RoundInfo.IsComputerWins)
        return "Computer";

    else 
        return "No Winner";

}

string GetFinalWinner(const strGameInfo& Info)
{
    if (Info.PlayerWon > Info.ComputerWon)
        return "Player";

    else if (Info.ComputerWon > Info.PlayerWon)
        return "Computer";

    else
        return "No Winner";

}

void RoundResult(strGameInfo& Info, short Counter)
{
    string PlayerChoice = ConvertEnumToStr(Info.RoundInfo.PlayerChoice);
    string ComputerChoice = ConvertEnumToStr(Info.RoundInfo.ComputerChoice);

    ScreenColor(Info);
    cout << "\n______Round["<< Counter<< "]______\n";

    cout << "Player Choice: " << PlayerChoice << endl;
    cout << "Computer Choice: " << ComputerChoice << endl;
    cout << "Round Winner: " << GetRoundWinner(Info) << endl;

    cout << "\n__________________________________\n";

}

void FinalHeader()
{
    cout << "\n";
    cout << "\t\t\t_________________________________________\n\n";
    cout << "\t\t\t            +++ Game Over +++            \n";
    cout << "\t\t\t_________________________________________\n\n";
    cout << "\t\t\t______________[Game Result]______________\n\n";
    cout << "\t\t\t_________________________________________"<<endl;
}

void GameResult(const strGameInfo& Info)
{
    FinalHeader();
    cout << "\t\t\tGame Rounds:\n";
    cout << "\t\t\tPlayer Won Times: " << Info.PlayerWon << endl;
    cout << "\t\t\tComputer Won Times: " << Info.ComputerWon << endl;
    cout << "\t\t\tDraw Times: " << Info.DrawTimes << endl;
    cout << "\t\t\tFinal Winner: " << GetFinalWinner(Info) << endl;
    cout << "\t\t\t_____________________________\n";
}

void ResetTheGame(strGameInfo& Info)
{
    Info.ComputerWon = 0;
    Info.PlayerWon = 0;
    Info.DrawTimes = 0;
}

void PlayGame()
{
    short Rounds = 0, Counter = 0;
    strGameInfo GameInfo;
    char ContinuePlaying = ' ';
    bool ClearScreen = false;

    do {
        
        if (ClearScreen) {
            system("color 0F");
            system("cls");
        }
            

            Rounds = ReadNumberInRange("How many rounds do you want to play from (1-10):\n", 1, 10);
        
        do {
            ++Counter;

            cout << "\nRound[" << Counter << "] begins:\n";
            GameMenu();

            short PlayerChoice = ReadNumberInRange("Choose a number between(1-3)\n", 1, 3);
            short ComputerChoice = RandomNumberInRange(1, 3);

            GameInfo.RoundInfo.PlayerChoice = NumberToEnum(PlayerChoice);
            GameInfo.RoundInfo.ComputerChoice = NumberToEnum(ComputerChoice);

            DecideRoundWinner(GameInfo);
            RoundResult(GameInfo, Counter);
            ResetRound(GameInfo);
            --Rounds;
        } while (Rounds > 0);

        GameResult(GameInfo);

        Counter = 0;
        ResetTheGame(GameInfo);

        cout << "Do you want to continue playing?(Y/N)?\n";
        cin >> ContinuePlaying;

        ClearScreen = true;
        
    } while (ContinuePlaying == 'Y' || ContinuePlaying == 'y');

    cout << "\nSorry to see you leave, have a nice day\n";

}


int main()
{
    srand((unsigned)time(NULL));
    PlayGame();
    



    return 0;
}

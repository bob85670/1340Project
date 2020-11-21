//# 1340Project
#include <iostream>
#include <string>
#include <cstdlib>
#include <ctime>
#include <iomanip>
using namespace std;

//global variables
bool checkwin_game2(string);
string get_random_number();
int get_outplaced_matched_count(string, string);
int get_inplaced_matched_count(string, string);

//Denote Players' name and marks
struct records {
  string name;
  int marks;
  int count_in_game1;
  int count_in_game3;
};
records record[2];

//Game1
int Guess_a_number(){
  int num, guess, count = 0, upperbound = 100, lowerbound = 1;
  bool CorrectAnswer;

  //The number is random
  srand(time(NULL));
  num = rand() % 100 + 1;

  CorrectAnswer = false;

  //keep guessing until is CorrectAnswer
  while (!CorrectAnswer){
    cout << lowerbound << " - " << upperbound << endl;
    cout << "I guess the number is ";
    cin >> guess;

    if (guess == num){
      cout << "It is Correct! You have used " << count << " trials." << endl;
      CorrectAnswer = true;
    }
    else if (guess < num){
      cout << "It is too small. Let's try again" <<endl;
      lowerbound = guess;
      count ++;
    }
    else{
      cout << "It is too large. Let's try again" << endl;
      upperbound = guess;
      count ++;
    }
  }
  return count;
}

//Game2
int Tic_tac_toe(){

  //Make the board
  int pos;
  string board = "012\n345\n678";
  cout << board <<endl;


  //Take turns until 4 times
  for (int i = 0; i < 4; i++)
  {

    //X first, and check win
    cout << "X--> ";
    cin >> pos;
    if (pos == 3 or pos ==4 or pos == 5){   //adjust the board 
        pos++;
    }
    else if (pos == 3 or pos ==4 or pos == 5){
        pos = pos + 2;
    }
    board = board.replace(pos, 1, "X");
    cout << board << endl;

    if (checkwin_game2(board) == true){
      cout << "Winner: X" << endl;
      return 'X';
    }

    //O next, and check win
    cout << "O--> ";
    cin >> pos;
    if (pos == 3 or pos ==4 or pos == 5){    //adjust the board
        pos++;
    }else if (pos == 3 or pos ==4 or pos == 5){
        pos = pos + 2;
    }
    board = board.replace(pos, 1, "O");
    cout << board << endl;

    if (checkwin_game2(board) == true){
      cout << "Winner: O" <<endl;
      return 'O';
    }
  }

  //The last move(9th times) by X: win or tie
  cout << board <<endl;
  cout << "X--> ";
  cin >> pos;
  if (pos == 3 or pos ==4 or pos == 5){   //adjust the board
    pos++;
  }else if (pos == 3 or pos ==4 or pos == 5){
    pos = pos + 2;
  }
  board = board.replace(pos, 1, "X");
  if (checkwin_game2(board) == true){
    cout << "Winner: X" <<endl;
    return 'X';
  }else{
    cout << "Winner: None." << endl;
    return 'N';
  }
}

//Used in Game2: to check win
bool checkwin_game2(string board){
  if ((board[0]==board[1] and board[1]==board[2]) or
     (board[4]==board[5] and board[5]==board[6]) or
     (board[8]==board[9] and board[9]==board[10]) or
     (board[0]==board[4] and board[4]==board[8]) or
     (board[1]==board[5] and board[5]==board[9]) or
     (board[2]==board[6] and board[6]==board[10]) or
     (board[0]==board[5] and board[5]==board[10]) or
     (board[2]==board[5] and board[5]==board[8])){
       return true;
     }
}

//Game 3: MaterMind

int master_mind(){
  string actual = "";
  actual = get_random_number();

  cout << "                              In place  Out of place " << endl;
  cout << "                              ---------- ------------" << endl;
  string guess;
  for (int i = 1; i <= 50; i++) {
    int score = i;
    cout << i << ". Your guess: ";
    cin >> guess;
    cout << "       you entered: " << guess;
    int inplace_count = get_inplaced_matched_count(actual, guess);
    int outplace_count = get_outplaced_matched_count(actual, guess);
    cout << "           " << inplace_count << "         " << outplace_count
         << endl;
    if (inplace_count == 4) {
      cout << "You have done " << score << " trials." << endl;
      return score;
      break;
    }

    if (i == 50) {
      cout << endl << "   Better luck next time." << endl;
      return score;
    }
  }
}

//Generating the 4-digit answer of the game(The 4 digits are all different from each other)
string get_random_number() {
  srand(time(0));
  string num = "";
  while (1) {
    int n1 = rand() % 9;
    int n2 = rand() % 9;
    int n3 = rand() % 9;
    int n4 = rand() % 9;
    if (n1 == n2 || n1 == n3 || n1 == n4 || n2 == n1 || n2 == n3 || n2 == n4 || n3 == n1 || n3 == n2 || n3 == n4)
      continue;
    else {
      num += to_string(n1);
      num += to_string(n2);
      num += to_string(n3);
      num += to_string(n4);
      return num;
    }
  }
}

//Check and output how many digits that the player guessed in place
int get_inplaced_matched_count(string actual, string guessed) {
  if (actual == guessed) {
    return 4;
  }
  int c = 0;
  if (actual[0] == guessed[0])
    c++;
  if (actual[1] == guessed[1])
    c++;
  if (actual[2] == guessed[2])
    c++;
  if (actual[3] == guessed[3])
    c++;
  return c;
}

//Check and output how many digits that the player guessed out of place
int get_outplaced_matched_count(string actual, string guessed) {
  int c = 0;
  for (int i = 0; i < actual.length(); i++) {
    for (int j = 0; j < guessed.length(); j++) {
      if (j == i)
        continue;
      if (actual[i] == guessed[j]) {
        c++;
        break;
      }
    }
  }
  return c;
}
//End of mastermind

int main()
{
  //char choice;
  int whofirst, whonext, i, choice;

  //Greating and enter names
  cout << "Hello! Welcome to Battle Royale!"<< endl;
  cout << "Now, enter you name." << endl;
  for (i = 0; i < 2; i++ ){
    cout << "Name of Player " << i + 1 << ": ";
    cin >> record[i].name;
  }
  cout << record[0].name << " and " << record[1].name << ", win mini-games and get 100 points as fast as you can!" << endl;

  //Keep running until one reaches 100 points
  while ((record[0].marks <= 100) or (record[1].marks <= 100))
  {
    cout << "Current Score " << endl;
    cout << record[0].name << ": " << record[0].marks << endl;
    cout << record[1].name << ": " << record[1].marks << endl;
    if ((record[0].marks >= 100) or (record[1].marks >= 100)){
        break;
    }

    //decide who play first by randomness
    srand(time(NULL));
    whofirst = rand() % 1;

    if (whofirst == 1){
      whonext = 0;
    }else{
      whonext = 1;
    }

    //choose play which game
    cout << "Game 1: Guess a number(10 marks)  Game 2: Tic-tac-toe(20 marks)  Game 3: Master-Mind(20 marks)" << endl;
    cout << "Choose the Game: Game ";
    cin >> choice;

    //implement Game1,2,3 function and declare who wins marks
    switch (choice)
    {
      case 1:
        record[whofirst].count_in_game1 = Guess_a_number();
        record[whonext].count_in_game1 = Guess_a_number();

        if (record[0].count_in_game1 > record[1].count_in_game1){
          cout << record[1].name << " wins! " <<endl;
          record[1].marks = record[1].marks + 10;
          break;
        }
        else{
          cout << record[0].name << " wins! "<< endl;
          record[0].marks = record[0].marks + 10;
          break;
        }

      case 2:
        if (Tic_tac_toe() == 'X'){
          cout << record[whofirst].name << " wins! " <<endl;
          record[whofirst].marks = record[whofirst].marks + 20;
          break;
        }else if (Tic_tac_toe() == 'O'){
          cout << record[whonext].name << " wins! "<<endl;
          record[whonext].marks = record[whonext].marks + 20;
          break;
        }else{
          cout << "The game draws. No marks are rewarded";
          break;
        }

      case 3:
      record[whofirst].count_in_game3 = master_mind();
      record[whonext].count_in_game3 = master_mind();
      if (record[0].count_in_game3 > record[1].count_in_game3){
          cout << "Congratulations "<< record[1].name << ", you are the winner of MasterMind!!"<< endl;
          record[1].marks = record[1].marks + 20;
          break;
      }
      else if (record[0].count_in_game3 < record[1].count_in_game3){
          cout << "Congratulations "<< record[0].name << ", you are the winner of MasterMind!!"<< endl;
          record[0].marks = record[0].marks + 20;
          break;
      }
      else{
          cout << "The game draws" <<endl;
          record[0].marks = record[0].marks + 10;
          record[1].marks = record[1].marks + 10;
          break;
      }
        break;
    }
  }

  // when 100marks are reached, declare who is the winner ,games end
  if (record[0].marks >= 100){
    cout << "Congratulations "<< record[0].name << ", you win!" << endl;
    cout << "Don't be sad, " << record[1].name << "." << endl;
  }
  else if (record[1].marks >= 100){
    cout << "Congratulations "<< record[1].name << ", you win!" << endl;
    cout << "Don't be sad, " << record[0].name << "."<< endl;
  }

 }








#include "SFML/Graphics.hpp";
using namespace sf;
using namespace std;

int main() {
	RenderWindow window(VideoMode(1280, 720), "WarTanks");

	int nav = 0; bool pause = 0;//  navigation and pause game 

	Texture ti;
	ti.loadFromFile("intro.jpg");
	Sprite ibg;
	ibg.setTexture(ti);

	//$$$$$$$ the cover that holds the text inside it $$$$$$$$$
	Texture textcover;
	textcover.loadFromFile("TextCover.png");
	Sprite textCover[6];
	//############# Main menu ###############
	for (int i = 0; i < 3; i++)
	{
		textCover[i].setScale(1.3, 1);
		textCover[i].setTexture(textcover);
	}
	textCover[0].setPosition(0, 0);
	textCover[1].setPosition(0, 100);
	textCover[2].setPosition(0, 200);

	//############# Campaign menu ############## 
	for (int i = 3; i < 6; i++)
	{
		textCover[i].setScale(2, 2);
		textCover[i].setTexture(textcover);
	}
	textCover[3].setPosition(0, 0);
	textCover[4].setPosition(0, 200);
	textCover[5].setPosition(0, 400);

	//############ main menu text ############
	Font font;		  Text mmt[6];		bool color = 0;
	font.loadFromFile("Top Secret Stamp.ttf");

	for (int i = 0; i < 3; i++)
	{
		mmt[i].setFont(font);
		mmt[i].setCharacterSize(48);
		mmt[i].setStyle(Text::Bold);
	}

	mmt[0].setString("Campaign");			mmt[1].setString("Survival");		mmt[2].setString("Exit");
	mmt[0].setPosition(47, 15);				mmt[1].setPosition(65, 115);		mmt[2].setPosition(100, 215);

	for (int i = 3; i < 6; i++) // text in campaign menu
	{
		mmt[i].setFont(font);
		mmt[i].setCharacterSize(50);
		mmt[i].setStyle(Text::Bold);
	}
	mmt[3].setString("Scorched Earth");			mmt[4].setString("Ashes To Ashes");		mmt[5].setString("End of the Line");
	mmt[3].setPosition(67, 70);					mmt[4].setPosition(67, 270);			mmt[5].setPosition(67, 470);



	while (window.isOpen())
	{
		Event event;
		while (window.pollEvent(event))
		{
			if (event.type == Event::Closed)
				window.close();
		}
					//################### change text color in menus ###################
		Vector2i mousepos = Mouse::getPosition(window);//to get position of mouse relative to window "easier than {Mouse ms;}"
		// change color of campaign text
		{
			if (mousepos.x > 45 && mousepos.x < 255 && mousepos.y>25 && mousepos.y < 76 && nav == 0)
				color = 1;
			else 			color = 0;

			if (!color)		mmt[0].setFillColor(Color::White);
			if (color)		mmt[0].setFillColor(Color::Blue);
		}
		// change color of survival text
		{
			if (mousepos.x > 67 && mousepos.x < 237 && mousepos.y>125 && mousepos.y < 164 && nav == 0)
				color = 1;
			else 			color = 0;

			if (!color)		mmt[1].setFillColor(Color::White);
			if (color)		mmt[1].setFillColor(Color::Blue);
		}
		// change color of exit text
		{
			if (mousepos.x > 100 && mousepos.x < 190 && mousepos.y>227 && mousepos.y < 268 && nav == 0)
				color = 1;
			else            color = 0;

			if (!color)		mmt[2].setFillColor(Color::White);
			if (color)		mmt[2].setFillColor(Color::Blue);
		}
		// change color of scorched earth text
		{
			if (mousepos.x > 67 && mousepos.x < 393 && mousepos.y>78 && mousepos.y < 120 && nav == 1)
				color = 1;
			else 			color = 0;

			if (!color)		mmt[3].setFillColor(Color::White);
			if (color)		mmt[3].setFillColor(Color::Blue);
		}
		// change color of ashes to ashes text
		{
			if (mousepos.x > 67 && mousepos.x < 390 && mousepos.y>278 && mousepos.y < 320 && nav == 1)
				color = 1;
			else 			color = 0;

			if (!color)		mmt[4].setFillColor(Color::White);
			if (color)		mmt[4].setFillColor(Color::Blue);
		}
		// change color of end of the line text
		{
			if (mousepos.x > 67 && mousepos.x < 407 && mousepos.y>478 && mousepos.y < 520 && nav == 1)
				color = 1;
			else            color = 0;

			if (!color)		mmt[5].setFillColor(Color::White);
			if (color)		mmt[5].setFillColor(Color::Blue);
		}

		//################# Navigation ##################
		if (Mouse::isButtonPressed(Mouse::Left))
		{
			if (mousepos.x > 45 && mousepos.x < 280 && mousepos.y > 25 && mousepos.y < 80 && nav == 0)   nav = 1; //from main menu to campaign "click on campaign"
			if (mousepos.x > 67 && mousepos.x < 237 && mousepos.y>125 && mousepos.y < 164 && nav == 0)  nav = 2; //from main menu to survival "click on survival"
			if (mousepos.x > 100 && mousepos.x < 190 && mousepos.y>227 && mousepos.y < 268 && nav == 0) window.close(); //exit from main menu "click on exit"
			if (mousepos.x > 67 && mousepos.x < 393 && mousepos.y>78 && mousepos.y < 120 && nav == 1) nav = 11;//play scorched earth  "1st level"
			if (mousepos.x > 67 && mousepos.x < 390 && mousepos.y>278 && mousepos.y < 320 && nav == 1)nav = 12;//play ashes to ashes  "2nd level"
			if (mousepos.x > 67 && mousepos.x < 407 && mousepos.y>478 && mousepos.y < 520 && nav == 1)nav = 13;//play end of the line "3rd level"
		}
		// ################ pause menu and go back ###########
		if (Keyboard::isKeyPressed(Keyboard::Escape))
		{
			if (nav == 1)nav = 0;// go from campaign menu to main menu
			if (nav == 2 || nav == 11 || nav == 12 || nav == 13) bool pause = 1; //pause the game

		}
		if (pause)
			RenderWindow pause(VideoMode(640, 360), "Pause Menu");



		//#################### draw ###################
		window.clear();
		if (nav == 0)
		{
			window.draw(ibg);
			for (int i = 0; i < 3; i++)
			{
				window.draw(textCover[i]);
				window.draw(mmt[i]);
			}

		}
		if (nav == 1)
		{
			window.draw(ibg);
			for (int i = 3; i < 6; i++)
			{
				window.draw(textCover[i]);
				window.draw(mmt[i]);
			}
		}
		window.display();
	}
	return 0;
}

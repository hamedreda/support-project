#include "SFML/Graphics.hpp" 
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
	textcover.loadFromFile("tank-game after ps.png");
	Sprite textCover[9]; // new added 3 more
	//############# Main menu ###############
	for (int i = 3; i < 9; i++) // new changed numbers and positions
	{
		textCover[i].setScale(1.3, 1);
		textCover[i].setTexture(textcover);
	}
	textCover[3].setPosition(0, 0);
	textCover[4].setPosition(0, 120);
	textCover[5].setPosition(0, 240);
	textCover[6].setPosition(0, 360);
	textCover[7].setPosition(0, 480);
	textCover[8].setPosition(0, 600);


	//############# Campaign menu ############## 
	for (int i = 0; i < 3; i++) //new changed numbers
	{
		textCover[i].setScale(2, 2);
		textCover[i].setTexture(textcover);
	}
	textCover[0].setPosition(0, 0);
	textCover[1].setPosition(0, 200);
	textCover[2].setPosition(0, 400);

	//############ main menu text ############
	Font font;		  Text mmt[9];		bool color = 0;
	font.loadFromFile("Top Secret Stamp.ttf");

	for (int i = 3; i < 9; i++) // new added text and changed positions
	{
		mmt[i].setFont(font);
		mmt[i].setCharacterSize(48);
		mmt[i].setStyle(Text::Bold);
	}

	mmt[3].setString("Campaign");			mmt[4].setString("Survival");		mmt[7].setString("Exit");
	mmt[3].setPosition(47, 15);				mmt[4].setPosition(65, 135);		mmt[7].setPosition(100, 495);
	mmt[5].setString("how to play");		mmt[6].setString("Credits");		mmt[8].setString("highscores");
	mmt[5].setPosition(43, 255);			mmt[6].setPosition(80, 375);		mmt[8].setPosition(43, 615);

	for (int i = 0; i < 3; i++) // text in campaign menu
	{
		mmt[i].setFont(font);
		mmt[i].setCharacterSize(50);
		mmt[i].setStyle(Text::Bold);
	}
	mmt[0].setString("Scorched Earth");			mmt[1].setString("Ashes To Ashes");		mmt[2].setString("End of the Line");
	mmt[0].setPosition(67, 70);					mmt[1].setPosition(67, 270);			mmt[2].setPosition(67, 470);

	//new
	// credits
	Text clowns[6];
	for (int i = 0; i < 6; i++) 
	{
		clowns[i].setFont(font);
		clowns[i].setCharacterSize(100);
		clowns[i].setStyle(Text::Bold);
	}
	clowns[0].setString("Hamed");				clowns[1].setString("Haitham");				clowns[2].setString("Andrew");
	clowns[0].setPosition(5, 5);				clowns[1].setPosition(640, 5);				clowns[2].setPosition(5, 205);
	clowns[3].setString("Abdeltawab");			clowns[4].setString("Faisal");				clowns[5].setString("Bebo");
	clowns[3].setPosition(640, 205);			clowns[4].setPosition(5, 405);				clowns[5].setPosition(640, 405);
	
	//new 
	// how to play 
	Texture how_to_play[7];
	how_to_play[0].loadFromFile("white-arrow-left.png");	how_to_play[1].loadFromFile("white-arrow-right.png");	
	how_to_play[2].loadFromFile("spacebar.png");			how_to_play[3].loadFromFile("apple.png");
	how_to_play[4].loadFromFile("shield.png");				how_to_play[5].loadFromFile("tbooster.png");
	how_to_play[6].loadFromFile("bbooster.png");			

	Sprite HowNav[2]; for (int i = 0; i < 2; i++) { HowNav[i].setTexture(how_to_play[i]); } // for navigation in how to play 
	HowNav[0].setScale(0.2, 0.1);		HowNav[0].setPosition(500, 650);
	HowNav[1].setScale(0.2, 0.1);		HowNav[1].setPosition(640, 650);

	Sprite HowtoPlay[7]; for (int i = 0; i < 7; i++) { HowtoPlay[i].setTexture(how_to_play[i]); }
	HowtoPlay[0].setScale(0.2,0.2 );		HowtoPlay[0].setPosition(5,180 );
	HowtoPlay[1].setScale(0.2,0.2 );		HowtoPlay[1].setPosition(65,180 );
	HowtoPlay[2].setScale(0.7,0.2 );		HowtoPlay[2].setPosition(5,340 );
	HowtoPlay[3].setScale(0.14,0.14 );		HowtoPlay[3].setPosition(40,100 );
	HowtoPlay[4].setScale(0.04, 0.04);		HowtoPlay[4].setPosition(30,220);
	HowtoPlay[5].setScale(0.03, 0.03);		HowtoPlay[5].setPosition(25,335 );
	HowtoPlay[6].setScale(0.06, 0.06);		HowtoPlay[6].setPosition(40,460 );
	/*HowtoPlay[7].setScale(, );		HowtoPlay[7].setPosition(, );
	HowtoPlay[8].setScale(, );		HowtoPlay[8].setPosition(, );
	HowtoPlay[9].setScale(, );		HowtoPlay[9].setPosition(, );
	*/
	Text htp21[4];
	for (int i = 0; i < 4; i++)
	{
		htp21[i].setFont(font);
		htp21[i].setCharacterSize(50);
		htp21[i].setStyle(Text::Bold);
	}
	htp21[0].setCharacterSize(100);		htp21[0].setFillColor(Color::Blue);
	htp21[0].setString("Movement");		htp21[1].setString("left & right arrows to move");		htp21[2].setString("spacebar to shoot");
	htp21[0].setPosition(400, 5);		htp21[1].setPosition(240, 200);							htp21[2].setPosition(240, 320);
	htp21[3].setString("TIP : always move or you will get hurt");			htp21[3].setPosition(200,590);

	Text htp22[5];
	for (int i = 0; i < 5; i++)
	{
		htp22[i].setFont(font);
		htp22[i].setCharacterSize(50);
		htp22[i].setStyle(Text::Bold);
	}
	htp22[0].setString("Campaign & Survival");											htp22[0].setPosition(350, 0);	htp22[0].setFillColor(Color::Blue);
	htp22[1].setString("200 is the required score to pass Scorched earth");				htp22[1].setPosition(5, 69);
	htp22[2].setString("250 is the required score to pass Ashes To Ashes");				htp22[2].setPosition(5, 169);
	htp22[3].setString("300 is the required score to pass End of the Line");			htp22[3].setPosition(5, 269);
	htp22[4].setString("challenge yourself in survival mode and set your highscore");	htp22[4].setPosition(5, 369);

	Text htp23[5];
	for (int i = 0; i < 5; i++)
	{
		htp23[i].setFont(font);
		htp23[i].setCharacterSize(50);
		htp23[i].setStyle(Text::Bold);
	}
	htp23[0].setString("Pickups");		htp23[0].setCharacterSize(80);		htp23[0].setPosition(520, 0);	htp23[0].setFillColor(Color::Blue);
	htp23[1].setString("apple : regenerates health");						htp23[1].setPosition(169, 100);
	htp23[2].setString("armor : puts shield on tank");						htp23[2].setPosition(169, 220);
	htp23[3].setString("accelrator : increase tank velocity");				htp23[3].setPosition(169, 340);
	htp23[4].setString("thunder : increase bullet velocity");				htp23[4].setPosition(169, 460);


	//new
	//pause menu text
	Text pmt[2];
	for (int i = 0; i < 2; i++)
	{
		pmt[i].setFont(font);
		pmt[i].setCharacterSize(48);
		pmt[i].setStyle(Text::Bold);
	}
	pmt[0].setString("Resume");		pmt[0].setPosition(0, 0);
	pmt[1].setString("Main Menu");	pmt[1].setPosition(0, 150);



	while (window.isOpen())
	{
		Event event;
		while (window.pollEvent(event))
		{
			if (event.type == Event::Closed)
				window.close();
		}

		Vector2i mousepos = Mouse::getPosition(window);//to get position of mouse relative to window "easier than {Mouse ms;}"
		// change color of campaign text
		{
			if (mousepos.x > 45 && mousepos.x < 255 && mousepos.y>25 && mousepos.y < 76 && nav == 0)
				color = 1;
			else 			color = 0;

			if (!color)		mmt[3].setFillColor(Color::White);
			if (color)		mmt[3].setFillColor(Color::Blue);
		}
		// change color of survival text // new changed numbers
		{
			if (mousepos.x > 67 && mousepos.x < 237 && mousepos.y>145 && mousepos.y < 184 && nav == 0)
				color = 1;
			else 			color = 0;

			if (!color)		mmt[4].setFillColor(Color::White);
			if (color)		mmt[4].setFillColor(Color::Blue);
		}
		// change color of how to play text //new
		{
			if (mousepos.x > 43 && mousepos.x < 274 && mousepos.y>265 && mousepos.y < 315 && nav == 0)
				color = 1;
			else 			color = 0;

			if (!color)		mmt[5].setFillColor(Color::White);
			if (color)		mmt[5].setFillColor(Color::Blue);
		}
		// change color of credits text //new
		{
			if (mousepos.x > 80 && mousepos.x < 231 && mousepos.y>385 && mousepos.y < 424 && nav == 0)
				color = 1;
			else 			color = 0;

			if (!color)		mmt[6].setFillColor(Color::White);
			if (color)		mmt[6].setFillColor(Color::Blue);
		}
		// change color of exit text // new vhanged numbers
		{
			if (mousepos.x > 100 && mousepos.x < 190 && mousepos.y>505 && mousepos.y < 545 && nav == 0)
				color = 1;
			else            color = 0;

			if (!color)		mmt[7].setFillColor(Color::White);
			if (color)		mmt[7].setFillColor(Color::Blue);
		}
		// change color of highscores text // new
		{
			if (mousepos.x > 43 && mousepos.x < 258 && mousepos.y>625 && mousepos.y < 665 && nav == 0)
				color = 1;
			else            color = 0;

			if (!color)		mmt[8].setFillColor(Color::White);
			if (color)		mmt[8].setFillColor(Color::Blue);
		}
		// change color of scorched earth text
		{
			if (mousepos.x > 67 && mousepos.x < 393 && mousepos.y>78 && mousepos.y < 120 && nav == 1)
				color = 1;
			else 			color = 0;

			if (!color)		mmt[0].setFillColor(Color::White);
			if (color)		mmt[0].setFillColor(Color::Blue);
		}
		// change color of ashes to ashes text
		{
			if (mousepos.x > 67 && mousepos.x < 390 && mousepos.y>278 && mousepos.y < 320 && nav == 1)
				color = 1;
			else 			color = 0;

			if (!color)		mmt[1].setFillColor(Color::White);
			if (color)		mmt[1].setFillColor(Color::Blue);
		}
		// change color of end of the line text
		{
			if (mousepos.x > 67 && mousepos.x < 407 && mousepos.y>478 && mousepos.y < 520 && nav == 1)
				color = 1;
			else            color = 0;

			if (!color)		mmt[2].setFillColor(Color::White);
			if (color)		mmt[2].setFillColor(Color::Blue);
		}
		//new
		if (nav == 21 || nav==23) { HowNav[1].setPosition(1140, 650); HowNav[0].setPosition(0, 650); }
		if (nav == 22){ HowNav[1].setPosition(640, 650); HowNav[0].setPosition(500, 650); }

		//################# Navigation ##################
		if (Mouse::isButtonPressed(Mouse::Left)) // new changed numbers according to change color and added new text
		{
			if (mousepos.x > 45 && mousepos.x < 280 && mousepos.y > 25 && mousepos.y < 80 && nav == 0)   nav = 1; //from main menu to campaign "click on campaign"
			if (mousepos.x > 67 && mousepos.x < 237 && mousepos.y>145 && mousepos.y < 184 && nav == 0)  nav = 2; //from main menu to survival "click on survival"
			if (mousepos.x > 80 && mousepos.x < 231 && mousepos.y>385 && mousepos.y < 424 && nav == 0) nav = 3; // from main menu to credits 
			if (mousepos.x > 43 && mousepos.x < 274 && mousepos.y>265 && mousepos.y < 315 && nav == 0) nav = 21; // from main menu to how to play "movement"
			if (mousepos.x > 1140 && mousepos.x < 1269 && mousepos.y>650 && mousepos.y < 699 && nav == 21) nav = 22;// from movement to campaign & survival
			if (mousepos.x > 500 && mousepos.x < 629 && mousepos.y>650 && mousepos.y < 699 && nav == 22) nav = 21;// from campaign & survival to movement
			if (mousepos.x > 640 && mousepos.x < 769 && mousepos.y>650 && mousepos.y < 699 && nav == 22) nav = 23;// from campaign & survival to pickups
			if (mousepos.x > 11 && mousepos.x < 140 && mousepos.y>650 && mousepos.y < 699 && nav == 23) nav = 22;// from pickups to campaign & survival
			if (mousepos.x > 43 && mousepos.x < 258 && mousepos.y>625 && mousepos.y < 665 && nav == 0) nav = 4; // highscores
			if (mousepos.x > 100 && mousepos.x < 190 && mousepos.y>505 && mousepos.y < 545 && nav == 0) window.close(); //exit from main menu "click on exit"
			if (mousepos.x > 67 && mousepos.x < 393 && mousepos.y>78 && mousepos.y < 120 && nav == 1) nav = 11;//play scorched earth  "1st level"
			if (mousepos.x > 67 && mousepos.x < 390 && mousepos.y>278 && mousepos.y < 320 && nav == 1)nav = 12;//play ashes to ashes  "2nd level"
			if (mousepos.x > 67 && mousepos.x < 407 && mousepos.y>478 && mousepos.y < 520 && nav == 1)nav = 13;//play end of the line "3rd level"
		}
		// ################ pause menu and go back###########
		if (Keyboard::isKeyPressed(Keyboard::Escape))
		{	//new added numbers
			if (nav == 1 || nav==3|| nav==4 || nav ==21 ||nav==22 ||nav==23)nav = 0;// go from campaign menu to main menu // new added credits and how to play and highscores
			if (nav == 2 || nav == 11 || nav == 12 || nav == 13)  pause = 1; //pause the game

		}
		// new pause menu loop
		if (pause)
		{
			RenderWindow pausewindow(VideoMode(480, 240), "Pause Menu");
				
			while (pausewindow.isOpen())
			{
				while (pausewindow.pollEvent(event))
				{
					if (event.type == Event::Closed) 
					{
						pausewindow.close();
						pause = 0;
					}
				}
				Vector2i mousepos1 = Mouse::getPosition(pausewindow);
				// change color of Resume text
				{
					if (mousepos1.x > 0 && mousepos1.x < 155 && mousepos1.y>11 && mousepos1.y < 50 )
						color = 1;
					else 			color = 0;

					if (!color)		pmt[0].setFillColor(Color::White);
					if (color)		pmt[0].setFillColor(Color::Blue);
				}
				// change color of Main Menu text
				{
					if (mousepos1.x > 0 && mousepos1.x <235 && mousepos1.y>160 && mousepos1.y < 197 )
						color = 1;
					else 			color = 0;

					if (!color)		pmt[1].setFillColor(Color::White);
					if (color)		pmt[1].setFillColor(Color::Blue);
				}
				if (Mouse::isButtonPressed(Mouse::Left))
				{
					if (mousepos1.x > 0 && mousepos1.x < 155 && mousepos1.y>11 && mousepos1.y < 50)  //resume game
					{
						pause = 0;
						pausewindow.close();
					}
					if (mousepos1.x > 0 && mousepos1.x < 235 && mousepos1.y>160 && mousepos1.y < 197)  //back to main menu
					{
						pause = 0;
						pausewindow.close();
						nav = 0;
					}

				}


				pausewindow.clear();
				pausewindow.draw(pmt[0]);
				pausewindow.draw(pmt[1]);
				pausewindow.display();

			}
		}


		//#################### draw ###################
		window.clear();
		if (nav == 0) // main menu
		{
			window.draw(ibg);
			for (int i = 3; i < 9; i++) // new changed numbers
			{
				window.draw(textCover[i]);
				window.draw(mmt[i]);
			}

		}
		if (nav == 1) // campaign menu
		{
			window.draw(ibg);
			for (int i = 0; i < 3; i++)
			{
				window.draw(textCover[i]);
				window.draw(mmt[i]);
			}
		}
		if (nav == 3)
		{
			for (int i = 0; i < 6; i++)
			{
				window.draw(clowns[i]);
			}
		}
		//new
		if (nav == 21)
		{
			window.draw(HowNav[1]);
			for (int i = 0; i < 3; i++)
			{
				window.draw(HowtoPlay[i]);
				window.draw(htp21[i]);
			}
			window.draw(htp21[3]);
		}
		if (nav == 22)
		{
			window.draw(HowNav[0]);	window.draw(HowNav[1]);
			for (int i = 0; i < 5; i++) 
			{
				window.draw(htp22[i]);
			}
		}
		if (nav == 23)
		{
			window.draw(HowNav[0]);
			for (int i = 0; i < 5; i++)
			{
				window.draw(htp23[i]);
			}
			for (int i = 3; i < 7; i++)
			{
				window.draw(HowtoPlay[i]);
			}
		}
		window.display();
	}
	return 0;
}

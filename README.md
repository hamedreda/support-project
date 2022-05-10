#include <SFML/Graphics.hpp>;
#include <SFML/Audio.hpp>;
using namespace sf;
using namespace std;

Sprite b1[10], b2[5], sh[3], ss;// spr arr FOR BOMBS& SHIELD,apple
void setter() {
	//bomb1
	for (int i = 0; i < 10; i++)
	{
		b1[i].setPosition(rand() % 1200, -rand() % 2000 * (rand() % 5 + 1));
	}
	//nuke
	for (int i = 0; i < 5; i++)
	{
		b2[i].setPosition(rand() % 1200, -rand() % 2000 * (rand() % 5 + 1));
	}
	//apple
	for (int i = 0; i < 3; i++)
	{
		sh[i].setPosition(rand() % 1200, -rand() % 2000 * (rand() % 5 + 1));
	}
	//SHIELD
	ss.setPosition(rand() % 1200, -5040);
}
RenderWindow window(VideoMode(1280, 720), "Tanks"); //WINDOW
int main()
{

	window.setFramerateLimit(40); //fps
	int reqscore = 200;
	bool bound = 1;	//for boundries 
	int nav = 0; bool pause = 0;//  navigation and pause game 
	int a = 250, b = 0; //for HEALTH,SHEILD BAR
	int h = 100, shi = 0, s = 0; //VARIABLES FOR HEALTH ,SHEILD ,SCORE  
	int tc = 1, bc = 1; //speed const.(boosters)
	Clock tbc, bbc; // clock for boosters
	bool fired = 0, shon = 0, tbb = 0, bbb = 0; //bools for bullet,sheild,boosters
	Clock survt; int reqtime = 25, speed = 0; //survival time&speed

	//sian..start..
	Clock animation1[10], animation2[5];
	int cnt1x[5], cnt1y[5]; int cnt2x[10], cnt2y[10];
	bool collided1[10] = { 0 }, collided2[5] = { 0 };
	Texture exp;
	exp.loadFromFile("explosion.png");
	Sprite exp1[10], exp2[5];
	for (int i = 0; i < 10; i++) {
		exp1[i].setTexture(exp);
		exp1[i].setTextureRect(IntRect(0, 0, 145, 150));
		exp1[i].setPosition(2000, 2000);
		exp1[i].setScale(1, 1);
	}
	for (int i = 0; i < 5; i++) {
		exp2[i].setTexture(exp);
		exp2[i].setTextureRect(IntRect(0, 0, 145, 150));
		exp2[i].setPosition(2000, 2000);
		exp2[i].setScale(1.5, 1.5);
	}
	//sian..end..

	Font f; f.loadFromFile("font.ttf"); //FONT
						// TEXT FOR SCORE & HEALTH & SHEILD :
	Text score, health, sheild;
	score.setFont(f);					health.setFont(f);					sheild.setFont(f);
	score.setString(to_string(s));	    health.setString(to_string(h));		sheild.setString(to_string(shi));
	score.setPosition(15, -5);		    health.setPosition(950, -10);		sheild.setPosition(950, 30);
	score.setFillColor(Color::Yellow);  health.setFillColor(Color::Yellow);
	score.setCharacterSize(50);		    health.setCharacterSize(50);		sheild.setCharacterSize(50);

	//################################ Textures & Sprites ###################################
		//1-tank
	Texture t;
	t.loadFromFile("Tank.png");
	Sprite s1(t);
	s1.setTexture(t);
	s1.setScale(0.2, 0.2);
	s1.setPosition(20, 600);
	//2-bullets
	Texture t4;
	t4.loadFromFile("bullet.png");
	Sprite s2(t4);
	s2.setTexture(t4);
	s2.setScale(0.025, 0.025);
	s2.setPosition(800, 720);

	// 3- BOMBS & SHIELD, APPLE , BOOSTERS
	Texture t1, t2, t3, t5, p1, p2;
	t1.loadFromFile("bomb1.png"); 	t2.loadFromFile("bomb2.png"); 	t3.loadFromFile("shield.png"); t5.loadFromFile("apple.png");
	p1.loadFromFile("tbooster.png");	p2.loadFromFile("bbooster.png");

	srand(time(0)); //for random positions 

	// ........................  BOMBS& SHIELD,apple  .................................
	 //bomb1
	for (int i = 0; i < 10; i++)
	{
		b1[i].setTexture(t1);
		b1[i].setPosition(rand() % 1200, -rand() % 2000 * (rand() % 5 + 1));
		b1[i].setScale(0.07, 0.07);
	}
	//nuke
	for (int i = 0; i < 5; i++)
	{
		b2[i].setTexture(t2);
		b2[i].setPosition(rand() % 1200, -rand() % 2000 * (rand() % 5 + 1));
		b2[i].setScale(0.1, 0.1);
	}
	//apple
	for (int i = 0; i < 3; i++)
	{
		sh[i].setTexture(t5);
		sh[i].setPosition(rand() % 1200, -1440 * (i + 1));
		sh[i].setScale(0.1, 0.1);
	}
	//sheild
	ss.setTexture(t3);
	ss.setPosition(rand() % 1200, -5040);
	ss.setScale(0.03, 0.03);
	//boosters
	Sprite tb(p1), bb(p2), tbi(p1), bbi(p2);//tank booster,bullet booster, icons
	tb.setPosition(rand() % 1200, -7200); 	bb.setPosition(rand() % 1200, -6480);
	tb.setScale(0.02, 0.02); bb.setScale(0.05, 0.05);
	tbi.setPosition(800, 12); bbi.setPosition(750, 15);
	tbi.setScale(0.01, 0.01);	bbi.setScale(0.025, 0.025);

	// 4- game's starting & ending txt + intro & game bg 
	Texture ti, tg;
	ti.loadFromFile("intro.jpg"); tg.loadFromFile("game.jpg");
	Sprite ibg, gbg;
	ibg.setTexture(ti);
	gbg.setTexture(tg);
	Font ending;
	ending.loadFromFile("endgame.ttf");
	Font moving;
	moving.loadFromFile("chunk.otf");
	Text strtgame;	Text endgame[4];  Text movegame;
	for (int i = 0; i < 4; i++) {
		endgame[i].setFont(ending);
		endgame[i].setCharacterSize(65);
		endgame[i].setPosition(300, 200);
		endgame[i].setFillColor(sf::Color::Red);
	}
	strtgame.setFont(ending);
	strtgame.setCharacterSize(50);
	strtgame.setPosition(200, 300);
	strtgame.setString("press ENTER when you are ready to play");
	endgame[0].setString("Well done soldier");
	endgame[1].setString("Mission failed succesfully");
	endgame[2].setPosition(300, 300);
	endgame[3].setPosition(300, 400);
	movegame.setFont(moving);
	movegame.setCharacterSize(57);
	movegame.setPosition(85, -10);
	movegame.setFillColor(sf::Color::Red);
	movegame.setString("Move to the right & Press enter");



	// 5-  the cover that holds the text inside it 
	Texture textcover;
	textcover.loadFromFile("TextCover.png");
	Sprite textCover[9]; //new added 3 more
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
	HowtoPlay[0].setScale(0.2, 0.2);		HowtoPlay[0].setPosition(5, 180);
	HowtoPlay[1].setScale(0.2, 0.2);		HowtoPlay[1].setPosition(65, 180);
	HowtoPlay[2].setScale(0.7, 0.2);		HowtoPlay[2].setPosition(5, 340);
	HowtoPlay[3].setScale(0.14, 0.14);		HowtoPlay[3].setPosition(40, 100);
	HowtoPlay[4].setScale(0.04, 0.04);		HowtoPlay[4].setPosition(30, 220);
	HowtoPlay[5].setScale(0.03, 0.03);		HowtoPlay[5].setPosition(25, 335);
	HowtoPlay[6].setScale(0.06, 0.06);		HowtoPlay[6].setPosition(40, 460);


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
	htp21[3].setString("TIP :think well before shooting :) ");			htp21[3].setPosition(200, 590);

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




	//####################### sound ######################
	//bullet
	SoundBuffer music2;
	music2.loadFromFile("shoot.wav");
	Sound shoot;
	shoot.setBuffer(music2);
	//explosion
	SoundBuffer music1;
	music1.loadFromFile("explosion.wav");
	Sound explosion;
	explosion.setBuffer(music1);
	//game
	Music music;
	music.openFromFile("Game audio1.wav");
	music.setLoop("Game audio1.wav");
	//music.play();


	while (window.isOpen())
	{
		RectangleShape r1(Vector2f(b, 20)); r1.setPosition(1005, 50); r1.setFillColor(Color{ 200,200,200 }); //SHEILD BAR
		RectangleShape r2(Vector2f(a, 20)); r2.setPosition(1005, 15); r2.setFillColor(Color::Red); //HEALTH BAR

		Event event; //to close window
		while (window.pollEvent(event))
		{
			if (event.type == Event::Closed)
				window.close();
		}
		Vector2i mousepos = Mouse::getPosition(window);//to get position of mouse relative to window "easier than {Mouse ms;}"

		//....................main..menu......
		if (nav == 0)
		{
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

			//################# Navigation from main menu ##################
			if (Mouse::isButtonPressed(Mouse::Left))
			{
				if (mousepos.x > 45 && mousepos.x < 280 && mousepos.y > 25 && mousepos.y < 80)   nav = 1; // to campaign "click on campaign"
				if (mousepos.x > 67 && mousepos.x < 237 && mousepos.y>145 && mousepos.y < 184) {
					nav = 2;
					survt.restart();
				}// to survival "click on survival"
				if (mousepos.x > 80 && mousepos.x < 231 && mousepos.y>385 && mousepos.y < 424) nav = 3; // from main menu to credits
				if (mousepos.x > 43 && mousepos.x < 274 && mousepos.y>265 && mousepos.y < 315) nav = 21; // from main menu to how to play "movement"
				if (mousepos.x > 43 && mousepos.x < 258 && mousepos.y>625 && mousepos.y < 665) nav = 4; // highscores
				if (mousepos.x > 100 && mousepos.x < 190 && mousepos.y>505 && mousepos.y < 545) window.close(); //exit from main menu "click on exit"
			}
		}
		//.................campaign..mode..menu......
		if (nav == 1)
		{
			// change color of scorched earth text
			{
				if (mousepos.x > 67 && mousepos.x < 393 && mousepos.y>78 && mousepos.y < 120)
					color = 1;
				else 			color = 0;

				if (!color)		mmt[3].setFillColor(Color::White);
				if (color)		mmt[3].setFillColor(Color::Blue);
			}
			// change color of ashes to ashes text
			{
				if (mousepos.x > 67 && mousepos.x < 390 && mousepos.y>278 && mousepos.y < 320)
					color = 1;
				else 			color = 0;

				if (!color)		mmt[4].setFillColor(Color::White);
				if (color)		mmt[4].setFillColor(Color::Blue);
			}
			// change color of end of the line text
			{
				if (mousepos.x > 67 && mousepos.x < 407 && mousepos.y>478 && mousepos.y < 520)
					color = 1;
				else            color = 0;

				if (!color)		mmt[5].setFillColor(Color::White);
				if (color)		mmt[5].setFillColor(Color::Blue);
			}
			//################# Navigation in campaign ##################
			if (Mouse::isButtonPressed(Mouse::Left))
			{
				if (mousepos.x > 67 && mousepos.x < 393 && mousepos.y>78 && mousepos.y < 120) nav = 11;//play scorched earth  "1st level"
				if (mousepos.x > 67 && mousepos.x < 390 && mousepos.y>278 && mousepos.y < 320)nav = 12;//play ashes to ashes  "2nd level"
				if (mousepos.x > 67 && mousepos.x < 407 && mousepos.y>478 && mousepos.y < 520)nav = 13;//play end of the line "3rd level"
			}
		}
		//...................How to Play...........
		if (nav == 21 || nav == 22 | nav == 23)
		{
			// arrows for navigation in how to play
			if (nav == 21 || nav == 23) { HowNav[1].setPosition(1140, 650); HowNav[0].setPosition(0, 650); }
			if (nav == 22) { HowNav[1].setPosition(640, 650); HowNav[0].setPosition(500, 650); }
			if (Mouse::isButtonPressed(Mouse::Left))
			{
				if (mousepos.x > 1140 && mousepos.x < 1269 && mousepos.y>650 && mousepos.y < 699 && nav == 21) nav = 22;// from movement to campaign & survival
				if (mousepos.x > 500 && mousepos.x < 629 && mousepos.y>650 && mousepos.y < 699 && nav == 22) nav = 21;// from campaign & survival to movement
				if (mousepos.x > 640 && mousepos.x < 769 && mousepos.y>650 && mousepos.y < 699 && nav == 22) nav = 23;// from campaign & survival to pickups
				if (mousepos.x > 11 && mousepos.x < 140 && mousepos.y>650 && mousepos.y < 699 && nav == 23) nav = 22;// from pickups to campaign & survival
			}
		}

		if (nav == 11)
		{
			if (s >= reqscore)
			{
				bound = 0;
				setter();
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter) && s1.getPosition().x >= 1280)
			{
				bound = 1;
				s1.setPosition(20, 600);
				s = 0;
				score.setString(to_string(s));
				nav = 12;
			}
		}
		if (nav == 12) {
			reqscore = 250;
			if (s >= reqscore)
			{
				bound = 0;
				setter();
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter) && s1.getPosition().x >= 1280)
			{
				bound = 1;
				s1.setPosition(20, 600);
				s = 0;
				score.setString(to_string(s));
				nav = 13;
			}
		}
		if (nav == 13)
		{
			reqscore = 300;
			if (s >= reqscore)
			{
				nav = 31;
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter) && s1.getPosition().x >= 1280)
			{
				bound = 1;
				s1.setPosition(20, 600);
				s = 0;
				score.setString(to_string(s));
			}
		}
		//.................game.....
		if (nav == 2 || nav == 11 || nav == 12 || nav == 13) {
			//######################## Movement ###############################
				//1-tank
			if (Keyboard::isKeyPressed(Keyboard::Right)) {
				s1.move(tc * 7, 0);
			}
			if (Keyboard::isKeyPressed(Keyboard::Left)) {
				s1.move(tc * -7, 0);
			}

			if (bound) {
				//2-bullet
				if (Keyboard::isKeyPressed(Keyboard::Space) && fired == 0) {
					fired = 1;
					s2.setPosition(s1.getPosition().x + 78 / 2.0, 600);
					shoot.play();
				}
				//repeats bullets's movement
				if (fired) {
					s2.move(0, -10 * bc);
				}

				if (s2.getPosition().y < 0) {
					fired = 0;
					s2.setPosition(800, 800);
				}

				//repeat bombs&powerups movement
				for (int i = 0; i < 10; i++) //bomb 1
				{
					if (b1[i].getPosition().y > 720)
						b1[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
				}
				for (int i = 0; i < 5; i++) //nuke
				{
					if (b2[i].getPosition().y > 720)
						b2[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
				}
				for (int i = 0; i < 3; i++) //apple
				{
					if (sh[i].getPosition().y > 720)
						sh[i].setPosition(rand() % 1200, -3600);

				}
				if (ss.getPosition().y > 720)//shield
					ss.setPosition(rand() % 1200, -5040);

				//move
				for (int i = 0; i < 10; i++) // bomb1
				{
					b1[i].move(0, 8 + speed);
				}
				for (int i = 0; i < 5; i++)  //nuke
				{
					b2[i].move(0, 8 + speed);
				}
				for (int i = 0; i < 3; i++) //apple
				{
					sh[i].move(0, 8);
				}
				ss.move(0, 8); //sheild
			}

			// #############################  collision  ########################
			//tank and window
			if (s1.getPosition().x <= 0 && Keyboard::isKeyPressed(Keyboard::Left)) {
				s1.move(tc * 7, 0);
			}
			if (s1.getPosition().x >= 1184.8 && Keyboard::isKeyPressed(Keyboard::Right) && bound == 1) {
				s1.move(tc * -7, 0);
			}

			//bombs with the tank
			for (int i = 0; i < 10; i++) {
				if (s1.getGlobalBounds().intersects(b1[i].getGlobalBounds())) {
					b1[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
					if (shon)
					{
						shi -= 10;
						sheild.setString(to_string(shi));
						b -= 25;
					}
					else {
						h -= 10;
						health.setString(to_string(h));
						a -= 25;
					}
					explosion.play();
				}
			}
			//nuke with tank
			for (int i = 0; i < 5; i++) {
				if (s1.getGlobalBounds().intersects(b2[i].getGlobalBounds())) {
					b2[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
					if (shon && shi < 30) {
						h -= 30 - shi;
						a -= 75 - b;
						health.setString(to_string(h));
						shi = 0;
						sheild.setString(to_string(shi));
						b = 0;
					}
					else if (shon)
					{
						shi -= 30;
						sheild.setString(to_string(shi));
						b -= 75;
					}
					else {
						h -= 30;
						health.setString(to_string(h));
						a -= 75;
					}
					explosion.play();
				}
			}
			//apple with the tank
			for (int i = 0; i < 3; i++) {
				if (s1.getGlobalBounds().intersects(sh[i].getGlobalBounds())) {
					sh[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
					if (h < 100)
						h += 10;
					health.setString(to_string(h));
					if (a < 250)
						a += 25;
				}
			}
			//shield with the tank
			if (s1.getGlobalBounds().intersects(ss.getGlobalBounds())) {
				ss.setPosition(rand() % 1200, -5040);
				shi += 50;
				if (shi > 100)
					shi = 100;
				sheild.setString(to_string(shi));
				b += 125;
				if (b > 250)
					b = 250;
				shon = 1;
			}

			//bullet with nukes
			for (int i = 0; i < 5; i++) {
				if (s2.getGlobalBounds().intersects(b2[i].getGlobalBounds())) {
					animation2[i].restart();
					exp2[i].setPosition(b2[i].getPosition().x - 100, b2[i].getPosition().y - 65);
					b2[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
					s += 10;
					score.setString(to_string(s));
					fired = 0;
					s2.setPosition(800, 800);
					collided2[i] = 1;
					explosion.play();
				}
			}
			//bullets with bombs
			for (int i = 0; i < 10; i++) {
				if (s2.getGlobalBounds().intersects(b1[i].getGlobalBounds())) {
					collided1[i] = 1;
					animation1[i].restart();
					exp1[i].setPosition(b1[i].getPosition().x - 50, b1[i].getPosition().y - 30);
					b1[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
					s += 10;
					score.setString(to_string(s));
					fired = 0;
					s2.setPosition(800, 800);
					explosion.play();
				}
			}

			//bullets with apple
			for (int i = 0; i < 3; i++)
			{
				if (s2.getGlobalBounds().intersects(sh[i].getGlobalBounds())) {
					sh[i].setPosition(rand() % 1200, -3600);
					fired = 0;
					s2.setPosition(1800, 800);
					explosion.play();
				}
			}
			//bullets with shields
			if (s2.getGlobalBounds().intersects(ss.getGlobalBounds())) {
				ss.setPosition(rand() % 1200, -5040);
				fired = 0;
				s2.setPosition(1800, 800);
				explosion.play();
			}
			//#################### explosion animation #########################
			for (int i = 0; i < 5; i++) {

				if (exp2[i].getPosition().x <= 1280 && exp2[i].getPosition().x >= -200) {
					if (collided2[i]) {
						if (animation2[i].getElapsedTime().asSeconds() > 0.1) {
							cnt1x[i]++;
							animation2[i].restart();
							if (cnt1x[i] % 3 == 2) {
								cnt1y[i] += 1;
							}
						}
					}

					cnt1x[i] %= 3;
					cnt1y[i] %= 3;
				}
				exp2[i].setTextureRect(IntRect(cnt1x[i] * 145, cnt1y[i] * 150, 145, 150));
				if (cnt1x[i] == cnt1y[i] && cnt1y[i] == 2) {
					exp2[i].setPosition(2000, 200);
					collided2[i] = 0;
					cnt1x[i] = 0;
					cnt1y[i] = 0;
				}

			}
			for (int i = 0; i < 10; i++) {

				if (exp1[i].getPosition().x <= 1280 && exp1[i].getPosition().x >= -200) {
					if (collided1[i]) {
						if (animation1[i].getElapsedTime().asSeconds() > 0.1) {
							cnt2x[i]++;
							animation1[i].restart();
							if (cnt2x[i] % 3 == 2) {
								cnt2y[i] += 1;
							}
						}
					}

					cnt2x[i] %= 3;
					cnt2y[i] %= 3;
				}
				exp1[i].setTextureRect(IntRect(cnt2x[i] * 145, cnt2y[i] * 150, 145, 150));
				if (cnt2x[i] == cnt2y[i] && cnt2y[i] == 2) {
					exp1[i].setPosition(2000, 200);
					collided1[i] = 0;
					cnt2x[i] = 0;
					cnt2y[i] = 0;
				}

			}


		}
		//.................survival..mode.......
		if (nav == 2) {
			//....repeat movement
			if (tb.getPosition().y > 720)//tank booster
				tb.setPosition(rand() % 1200, -7200);

			if (bb.getPosition().y > 720)//bullet booster
				bb.setPosition(rand() % 1200, -6480);
			//....move
			tb.move(0, 8); //tank booster
			bb.move(0, 8); //bullet booster
						// #############################  collision  ########################
			//tank booster with tank
			if (s1.getGlobalBounds().intersects(tb.getGlobalBounds())) {
				tb.setPosition(rand() % 1200, -6480);
				tc = 2;
				tbb = 1;
				tbc.restart();
			}
			//bullet booster with tank
			if (s1.getGlobalBounds().intersects(bb.getGlobalBounds())) {
				bb.setPosition(rand() % 1200, -6480);
				bc = 2;
				bbb = 1;
				bbc.restart();
			}
			//bullets with tank booster
			if (s2.getGlobalBounds().intersects(tb.getGlobalBounds())) {
				tb.setPosition(rand() % 1200, -6480);
				fired = 0;
				s2.setPosition(1800, 800);
				explosion.play();
			}
			//bullets with bullet booster
			if (s2.getGlobalBounds().intersects(bb.getGlobalBounds())) {
				bb.setPosition(rand() % 1200, -6480);
				fired = 0;
				s2.setPosition(1800, 800);
				explosion.play();
			}
			//################  boosters effect  #################
			if (tbb) {
				if (tbc.getElapsedTime().asSeconds() > 10) {
					tc = 1;
					tbb = 0;
				}
			}
			if (bbb) {
				if (bbc.getElapsedTime().asSeconds() > 10) {
					bc = 1;
					bbb = 0;
				}
			}
			if (survt.getElapsedTime().asSeconds() > reqtime) {
				speed += 2;
				reqtime += 25;
			}
		}

		if (h <= 0) {
			if (nav == 2)
				nav = 32;
			if (nav == 11 | nav == 12 | nav == 13)
				nav = 31;
		}


		if (shi <= 0)
			shon = 0;

		// ################ pause menu and go back###########
		if (Keyboard::isKeyPressed(Keyboard::Escape))
		{	//new added numbers
			if (nav == 1 || nav == 3 || nav == 4 || nav == 21 || nav == 22 || nav == 23)nav = 0;// go from campaign menu to main menu // new added credits and how to play and highscores
			if (nav == 2 || nav == 11 || nav == 12 || nav == 13)  pause = 1; //pause the game
			if (nav == 32 || nav == 31) { h = 100; nav = 0; survt.restart(); } // after survival go back to main menu

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
					if (mousepos1.x > 0 && mousepos1.x < 155 && mousepos1.y>11 && mousepos1.y < 50)
						color = 1;
					else 			color = 0;

					if (!color)		pmt[0].setFillColor(Color::White);
					if (color)		pmt[0].setFillColor(Color::Blue);
				}
				// change color of Main Menu text
				{
					if (mousepos1.x > 0 && mousepos1.x < 235 && mousepos1.y>160 && mousepos1.y < 197)
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
						main();
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
		//main menu
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
		if (nav == 3) //credits
		{
			for (int i = 0; i < 6; i++)
			{
				window.draw(clowns[i]);
			}
		}
		//new how to play "movement"
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
		if (nav == 22) // how to play " campaign and survival"
		{
			window.draw(HowNav[0]);	window.draw(HowNav[1]);
			for (int i = 0; i < 5; i++)
			{
				window.draw(htp22[i]);
			}
		}
		if (nav == 23)// how to play "pickups"
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





		//####### game  #######
		if (nav == 2 || nav == 11 || nav == 12 || nav == 13)
		{
			window.draw(gbg);
			window.draw(s1);
			window.draw(s2);

			if (bound) {
				for (int i = 0; i < 10; i++) //bomb 1
				{
					window.draw(b1[i]);
				}
				for (int i = 0; i < 5; i++) //bomb 2
				{
					window.draw(b2[i]);
				}
				for (int i = 0; i < 3; i++) //apple
				{
					window.draw(sh[i]);
				}
				window.draw(ss);//sheild
				window.draw(r1);// sheild bar
				window.draw(r2);// health bar
				// text for score health sheild
				window.draw(score);	window.draw(health);
				if (shon)
					window.draw(sheild);

				//nuke explosion
				for (int i = 0; i < 5; i++) {
					window.draw(exp2[i]);
				}
				//bomb explosion
				for (int i = 0; i < 10; i++) {
					window.draw(exp1[i]);
				}
			}
		}
		if (nav == 2)
		{	//boosters & boosters' icons
			window.draw(tb); 	window.draw(bb);
			if (tbb)
				window.draw(tbi);
			if (bbb)
				window.draw(bbi);
		}
		//#######  ending  #######
		endgame[2].setString("Score: " + to_string(s));
		endgame[3].setString("Time: delete this & put time");
		if (nav == 31)
		{
			if (s >= 300)
				window.draw(endgame[0]);
			if (h <= 0) {
				window.draw(endgame[1]);
				window.draw(endgame[2]);
			}
		}
		if (nav == 32) {
			window.draw(endgame[2]);
			window.draw(endgame[3]);
		}
		if (s >= reqscore) {
			if (nav == 11 || nav == 12 || nav == 13) {
				window.draw(movegame);
			}
		}

		window.display();
	}
	return 0;
}

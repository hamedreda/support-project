#include <SFML/Graphics.hpp>;
#include <SFML/Audio.hpp>;
using namespace sf;
using namespace std;
int h = 100, shi = 0, s = 0, a = 250, b = 0, pass = 0; //VARIABLES FOR HEALTH ,SHEILD ,SCORE ,HEALTH,SHEILD BAR
bool passed[2] = {};

// tebo
Sprite b1[10], b2[5], sh[3], ss;// spr arr FOR BOMBS& SHIELD,apple
void setter() { 
	for (int i = 0; i < 10; i++) { b1[i].setPosition(rand() % 1200, -rand() % 2000 * (rand() % 5 + 1)); }//bomb
	for (int i = 0; i < 5; i++) { b2[i].setPosition(rand() % 1200, -rand() % 2000 * (rand() % 5 + 1)); } //nuke
	for (int i = 0; i < 3; i++) { sh[i].setPosition(rand() % 1200, -rand() % 2000 * (rand() % 5 + 1)); } //SHIELD 
	ss.setPosition(rand() % 1200, -5040); shi = 0; s = 0; b = 0;//apple 
}
Music music;
//################################################# 
RenderWindow window(VideoMode(1280, 720), "Tanks"); //WINDOW
int main()
{
	Font f; f.loadFromFile("font.ttf"); //FONT
	window.setFramerateLimit(40); //fps //random movement in the background 
	bool random = 1; int mode = 0; Clock movement; //andrew
	bool shon = 0; //bool for sheild
	RectangleShape r1(Vector2f(b, 20)); r1.setPosition(1005, 50); r1.setFillColor(Color{ 200,200,200 }); //SHEILD BAR 
	RectangleShape r2(Vector2f(a, 20)); r2.setPosition(1005, 15); r2.setFillColor(Color::Red); //HEALTH BAR //#################################################

	int nav = 0; bool pause = 0;//bebo....navigation and pause game

	bool bound = 1;	//tebo..for boundries
	//hamed
	int reqscore = 200;
	int tc = 1, bc = 1; //speed const.(boosters)
	Clock tbc, bbc; // clock for boosters
	bool fired = 0, tbb = 0, bbb = 0; //bools for bullet,boosters
	Clock survt; int reqtime = 25, speed = 0; //survival time&speed
	Text survtt;	survtt.setFont(f); survtt.setCharacterSize(50);	survtt.setPosition(300, -5);
	int survcnt = 0;
	//#################################################

	//haitham
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
	//#################################################


	//andrew
							// TEXT FOR SCORE & HEALTH & SHEILD :
	Text score, health, sheild;
	score.setFont(f);					health.setFont(f);					sheild.setFont(f);
	score.setString(to_string(s));	    health.setString(to_string(h));		sheild.setString(to_string(shi));
	score.setPosition(15, -5);		    health.setPosition(950, -10);		sheild.setPosition(950, 30);
	score.setFillColor(Color::Yellow);  health.setFillColor(Color::Yellow);
	score.setCharacterSize(50);		    health.setCharacterSize(50);		sheild.setCharacterSize(50);
	//#################################################

	//hamed
	//1-tank
	Texture t;
	t.loadFromFile("Tank.png");
	Sprite s1(t);
	s1.setScale(0.2, 0.2);
	s1.setPosition(20, 600);
	//2-bullets
	Texture t4;
	t4.loadFromFile("bullet.png");
	Sprite s2(t4);
	s2.setScale(0.025, 0.025);
	s2.setPosition(800, 720);
	//#################################################

	//tebo
	srand(time(0)); //for random positions 

	// 3- BOMBS & SHIELD, APPLE , BOOSTERS
	Texture t1, t2, t3, t5, p1, p2;
	t1.loadFromFile("bomb1.png"); 	t2.loadFromFile("bomb2.png"); 	t3.loadFromFile("shield.png"); t5.loadFromFile("apple.png");
	p1.loadFromFile("tbooster.png");	p2.loadFromFile("bbooster.png");
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
		sh[i].setScale(0.25, 0.25);
	}
	//sheild
	ss.setTexture(t3);
	ss.setPosition(rand() % 1200, -5040);
	ss.setScale(0.03, 0.03);
	//#################################################

	//hamed
	//boosters
	Sprite tb(p1), bb(p2), tbi(p1), bbi(p2);//tank booster,bullet booster, icons
	tb.setPosition(rand() % 1200, -7200); 	bb.setPosition(rand() % 1200, -6480);
	tb.setScale(0.02, 0.02); bb.setScale(0.05, 0.05);
	tbi.setPosition(800, 12); bbi.setPosition(750, 15);
	tbi.setScale(0.01, 0.01);	bbi.setScale(0.025, 0.025);
	//#################################################


	//faisl

		// 4- game's starting & ending txt + intro & game bg 
	Texture ti, tg;
	ti.loadFromFile("intro.jpg"); tg.loadFromFile("game1.jpeg");
	Texture lvl1, lvl2, lvl3;
	lvl1.loadFromFile("lvl1.jpeg"); lvl2.loadFromFile("lvl2.jpeg"); lvl3.loadFromFile("lvl3.jpeg");
	Sprite ibg, gbg;
	ibg.setTexture(ti);
	Font ending;
	ending.loadFromFile("endgame.ttf");
	Font moving;
	moving.loadFromFile("chunk.otf");
	Text endgame[4];	Text backToMain;
	for (int i = 0; i < 4; i++) {
		endgame[i].setFont(ending);
		endgame[i].setCharacterSize(65);
		endgame[i].setPosition(300, 200);
		endgame[i].setFillColor(sf::Color::White);
	}
	endgame[0].setString("Well done soldier");
	endgame[1].setString("Mission failed succesfully");
	endgame[2].setPosition(300, 100);
	endgame[3].setPosition(300, 200);
	backToMain.setFont(ending);
	backToMain.setCharacterSize(50);
	backToMain.setFillColor(Color::White);
	backToMain.setString("press Esc to go back to Main Menu");
	backToMain.setPosition(300, 430);

	// 5-  the cover that holds the text inside it & lvl photos
	Texture bw[3], colored[3];
	bw[0].loadFromFile("lvl1bw.jpeg");
	bw[1].loadFromFile("lvl2bw.jpeg");
	bw[2].loadFromFile("lvl3bw.jpeg");
	colored[0].loadFromFile("lvl1colored.jpeg");
	colored[1].loadFromFile("lvl2colored.jpeg");
	colored[2].loadFromFile("lvl3colored.jpeg");
	Sprite lvlbw[3], lvlcolored[3];
	for (int i = 0; i < 3; i++) {
		lvlbw[i].setTexture(bw[i]);
		lvlcolored[i].setTexture(colored[i]);
	}
	Texture textcover;
	textcover.loadFromFile("TextCover.png");
	Sprite textCover[8];
	//############# Main menu ###############
	for (int i = 3; i < 8; i++)
	{
		textCover[i].setScale(1.3, 1);
		textCover[i].setTexture(textcover);
	}
	textCover[3].setPosition(0, 0);
	textCover[4].setPosition(0, 120);
	textCover[5].setPosition(0, 240);
	textCover[6].setPosition(0, 360);
	textCover[7].setPosition(0, 480);

	//############# Campaign menu ############## 
	for (int i = 0; i < 3; i++) //new changed numbers
	{
		textCover[i].setScale(2, 2);
		textCover[i].setTexture(textcover);
	}
	lvlbw[0].setPosition(0, 0);
	lvlbw[1].setPosition(0, 250);
	lvlbw[2].setPosition(0, 470);
	lvlcolored[0].setPosition(1600, 0);
	lvlcolored[1].setPosition(1600, 250);
	lvlcolored[2].setPosition(1600, 470);


	//############ main menu text ############
	Font font;		  Text mmt[8];		bool color = 0;
	font.loadFromFile("Top Secret Stamp.ttf");

	for (int i = 3; i < 8; i++)
	{
		mmt[i].setFont(font);
		mmt[i].setCharacterSize(48);
		mmt[i].setStyle(Text::Bold);
	}

	mmt[3].setString("Campaign");			mmt[4].setString("Survival");		mmt[7].setString("Exit");
	mmt[3].setPosition(47, 15);				mmt[4].setPosition(65, 135);		mmt[7].setPosition(100, 495);
	mmt[5].setString("how to play");		mmt[6].setString("Credits");
	mmt[5].setPosition(43, 255);			mmt[6].setPosition(80, 375);
	// text in campaign menu
	for (int i = 0; i < 3; i++)
	{
		mmt[i].setFont(font);
		mmt[i].setCharacterSize(50);
		mmt[i].setStyle(Text::Bold);
	}
	mmt[0].setString("Scorched Earth");			mmt[1].setString("Ashes To Ashes");		mmt[2].setString("End of the Line");
	mmt[0].setPosition(67, 70);					mmt[1].setPosition(67, 270);			mmt[2].setPosition(67, 470);

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

	// how to play 
	Texture htpbg;	htpbg.loadFromFile("how to play back ground2 resized.jpg");
	Sprite htpBG; htpBG.setTexture(htpbg);	htpBG.setScale(1, 1);	htpBG.setPosition(0, 0);

	Texture how_to_play[9];
	how_to_play[0].loadFromFile("white-arrow-left.png");	how_to_play[1].loadFromFile("white-arrow-right.png");
	how_to_play[2].loadFromFile("spacebar.png");			how_to_play[3].loadFromFile("apple.png");
	how_to_play[4].loadFromFile("shield.png");				how_to_play[5].loadFromFile("tbooster.png");
	how_to_play[6].loadFromFile("bbooster.png");			how_to_play[7].loadFromFile("bomb1.png");
	how_to_play[8].loadFromFile("bomb2.png");

	Sprite HowNav[2]; for (int i = 0; i < 2; i++) { HowNav[i].setTexture(how_to_play[i]); } // for navigation in how to play 
	HowNav[0].setScale(0.2, 0.1);		HowNav[0].setPosition(500, 650);
	HowNav[1].setScale(0.2, 0.1);		HowNav[1].setPosition(640, 650);

	Sprite HowtoPlay[9]; for (int i = 0; i < 9; i++) { HowtoPlay[i].setTexture(how_to_play[i]); }
	HowtoPlay[0].setScale(0.2, 0.2);		HowtoPlay[0].setPosition(5, 180);
	HowtoPlay[1].setScale(0.2, 0.2);		HowtoPlay[1].setPosition(65, 180);
	HowtoPlay[2].setScale(0.7, 0.2);		HowtoPlay[2].setPosition(5, 340);
	HowtoPlay[3].setScale(0.14, 0.14);		HowtoPlay[3].setPosition(40, 100);
	HowtoPlay[4].setScale(0.04, 0.04);		HowtoPlay[4].setPosition(30, 220);
	HowtoPlay[5].setScale(0.03, 0.03);		HowtoPlay[5].setPosition(25, 335);
	HowtoPlay[6].setScale(0.06, 0.06);		HowtoPlay[6].setPosition(40, 460);
	HowtoPlay[7].setScale(0.08, 0.08);		HowtoPlay[7].setPosition(40, 570);
	HowtoPlay[8].setScale(0.12, 0.12);		HowtoPlay[8].setPosition(700, 575);

	//movement
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

	//Campaign & Survival
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

	//Pickups
	Text htp23[7];
	for (int i = 0; i < 7; i++)
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
	htp23[5].setString("bomb : does 10 damage");							htp23[5].setPosition(150, 580);
	htp23[6].setString("nuke : does 30 damage");							htp23[6].setPosition(789, 580);

	//#################################################

	//bebo
	//pause menu  ^^^^
	Text pmt[3], options[3];//^^^^ added options array and pmt increased
	for (int i = 0; i < 3; i++)
	{
		pmt[i].setFont(font);
		pmt[i].setCharacterSize(48);
		pmt[i].setStyle(Text::Bold);
		options[i].setFont(font);
		options[i].setCharacterSize(48);
		options[i].setStyle(Text::Bold);
	}
	pmt[0].setString("Resume");		pmt[0].setPosition(0, 0);
	pmt[1].setString("Main Menu");	pmt[1].setPosition(0, 180);
	pmt[2].setString("Options");	pmt[2].setPosition(0, 90);//^^^^
	//^^^^
	options[0].setString("Music");		options[0].setPosition(10, 10);
	options[1].setString("Sound FX");	options[1].setPosition(10, 90);
	options[2].setString("press backspace to go back");	options[2].setPosition(0, 200);	options[2].setCharacterSize(30);
	Texture box, check; box.loadFromFile("empty-box1.png"); check.loadFromFile("check-mark.png");
	Sprite BOX1, BOX2, CHECK1, CHECK2;
	BOX1.setPosition(260, 10);	 BOX1.setScale(1, 1);		BOX1.setTexture(box);
	BOX2.setPosition(260, 90);	 BOX2.setScale(1, 1);		BOX2.setTexture(box);
	CHECK1.setPosition(265, 15);	 CHECK1.setScale(0.7, 0.7);	CHECK1.setTexture(check);
	CHECK2.setPosition(265, 95); CHECK2.setScale(0.7, 0.7);	CHECK2.setTexture(check);

	Texture pausemenu; pausemenu.loadFromFile("camo.jpg");
	Sprite PauseMenuBG; PauseMenuBG.setTexture(pausemenu);
	bool optsound = 1, optmusic = 1, optionsB = 0;

	//#################################################

	// haitham
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

	music.openFromFile("Game audio1.wav");
	music.setLoop("Game audio1.wav");
	music.play();
	//#################################################
	////arrowww
	Sprite arrow;
	Texture arrw;
	int zz = 0;
	arrw.loadFromFile("right arrow.png");
	arrow.setTexture(arrw);
	arrow.setTextureRect(IntRect(0, 0, 200, 188));
	arrow.setPosition(1100, 0);
	arrow.setScale(0.7, 0.7);
	bool levelup = 0;
	Clock arrowcnt;
	//########################################################
	//damage counter
	int dmgt = 0, bombcnt = 0, nukecnt = 0;
	//text for stats
	Text bombn, nuken, dmg;
	bombn.setFont(ending);
	bombn.setString("Number of bombs destroyed:" + to_string(bombcnt));
	bombn.setFillColor(Color::White);
	bombn.setPosition(300, 550);
	nuken.setFont(ending);
	nuken.setString("Number of nukes destroyed: " + to_string(nukecnt));
	nuken.setFillColor(Color::White);
	nuken.setPosition(300, 600);
	dmg.setFont(ending);
	dmg.setString("Damage taken: " + to_string(dmgt));
	dmg.setFillColor(Color::White);
	dmg.setPosition(300, 650);

	while (window.isOpen())
	{
		if (pass == 1)
			passed[0] = 1;
		if (pass == 2)
			passed[1] = 1;
		if (nav == 1) {
			a = 250, b = 0;
			h = 100, shi = 0, s = 0;
		}
		sheild.setString(to_string(shi));
		score.setString(to_string(s));
		health.setString(to_string(h));
		if (Keyboard::isKeyPressed(Keyboard::Right) || Keyboard::isKeyPressed(Keyboard::Left) || Keyboard::isKeyPressed(Keyboard::Space)) {
			random = 0;
			movement.restart();

		}
		if (movement.getElapsedTime().asSeconds() > 10) {
			random = 1;
		}
		if (nav == 0) {
			if (random) {
				//bool random int mode clock movemnt
				if (mode == 0) {
					s1.move(7, 0);
				}
				if (mode == 1) {
					s1.move(-7, 0);
				}

				if (movement.getElapsedTime().asSeconds() > 1) {
					mode = rand() % 3;
					movement.restart();
				}
				if (s1.getPosition().x <= 0 && mode == 1) {
					s1.move(tc * 7, 0);
				}
				if (s1.getPosition().x >= 1184.8 && mode == 0) {
					s1.move(tc * -7, 0);
				}
				if (mode == 2 && fired == 0) {
					fired = 1;
					s2.setPosition(s1.getPosition().x + 78 / 2.0, 600);
					shoot.play();
				}
				if (fired) {
					s2.move(0, -10 * bc);
				}

				if (s2.getPosition().y < 0) {
					fired = 0;
					s2.setPosition(800, 800);
				}
			}
		}
		r1.setSize(Vector2f(b, 20)); r2.setSize(Vector2f(a, 20));//andrew...resize.bars..

		Event event; //to close window
		while (window.pollEvent(event))
		{
			if (event.type == Event::Closed)
				window.close();
		}

		//bebo
		Vector2i mousepos = Mouse::getPosition(window);//to get position of mouse relative to window "easier than {Mouse ms;}"
		//....................main..menu......
		if (nav == 0)
		{
			// change color of campaign text
			{
				if (mousepos.x > 45 && mousepos.x < 255 && mousepos.y>25 && mousepos.y < 76)
					color = 1;
				else 			color = 0;

				if (!color)		mmt[3].setFillColor(Color::White);
				if (color)		mmt[3].setFillColor(Color::Blue);
			}
			// change color of survival text // new changed numbers
			{
				if (mousepos.x > 67 && mousepos.x < 237 && mousepos.y>145 && mousepos.y < 184)
					color = 1;
				else 			color = 0;

				if (!color)		mmt[4].setFillColor(Color::White);
				if (color)		mmt[4].setFillColor(Color::Blue);
			}
			// change color of how to play text //new
			{
				if (mousepos.x > 43 && mousepos.x < 274 && mousepos.y>265 && mousepos.y < 315)
					color = 1;
				else 			color = 0;

				if (!color)		mmt[5].setFillColor(Color::White);
				if (color)		mmt[5].setFillColor(Color::Blue);
			}
			// change color of credits text //new
			{
				if (mousepos.x > 80 && mousepos.x < 231 && mousepos.y>385 && mousepos.y < 424)
					color = 1;
				else 			color = 0;

				if (!color)		mmt[6].setFillColor(Color::White);
				if (color)		mmt[6].setFillColor(Color::Blue);
			}
			// change color of exit text // new vhanged numbers
			{
				if (mousepos.x > 100 && mousepos.x < 190 && mousepos.y>505 && mousepos.y < 545)
					color = 1;
				else            color = 0;

				if (!color)		mmt[7].setFillColor(Color::White);
				if (color)		mmt[7].setFillColor(Color::Blue);
			}


			//################# Navigation from main menu ##################
			if (Mouse::isButtonPressed(Mouse::Left))
			{
				if (mousepos.x > 45 && mousepos.x < 280 && mousepos.y > 25 && mousepos.y < 80) {
					movement.restart(); nav = 1;
				} // to campaign "click on campaign"
				if (mousepos.x > 67 && mousepos.x < 237 && mousepos.y>145 && mousepos.y < 184) {
					nav = 2;
					setter();
					s1.setPosition(20, 600);
					s2.setPosition(800, 720);
					h = 100;
					a = 250;
					survt.restart();
				}// to survival "click on survival"
				if (mousepos.x > 80 && mousepos.x < 231 && mousepos.y>385 && mousepos.y < 424) nav = 3; // from main menu to credits
				if (mousepos.x > 43 && mousepos.x < 274 && mousepos.y>265 && mousepos.y < 315) nav = 21; // from main menu to how to play "movement"

				if (mousepos.x > 100 && mousepos.x < 190 && mousepos.y>505 && mousepos.y < 545) window.close(); //exit from main menu "click on exit"
			}
		}
		//.................campaign..mode..menu......
		if (nav == 1)
		{
			// change color of scorched earth text
			{
				if (mousepos.y > 0 && mousepos.y < 250)
					color = 1;
				else 			color = 0;

				if (!color)		lvlcolored[0].setPosition(1600, 0);
				if (color)		lvlcolored[0].setPosition(0, 0);
			}
			// change color of ashes to ashes text
			{
				if (mousepos.y > 250 && mousepos.y < 470 && passed[0])
					color = 1;
				else 			color = 0;

				if (!color)		lvlcolored[1].setPosition(1600, 0);
				if (color)		lvlcolored[1].setPosition(0, 250);
			}
			// change color of end of the line text
			{
				if (mousepos.y > 470 && mousepos.y < 750 && passed[1])
					color = 1;
				else            color = 0;

				if (!color)		lvlcolored[2].setPosition(1600, 0);
				if (color)		lvlcolored[2].setPosition(0, 470);

			}
			//################# Navigation in campaign ##################
			if (movement.getElapsedTime().asSeconds() > 0.5)
				if (Mouse::isButtonPressed(Mouse::Left))
				{

					if (mousepos.y > 0 && mousepos.y < 250) { nav = 11; setter(); h = 100; a = 250; }//play scorched earth  "1st level"
					if (mousepos.y > 250 && mousepos.y < 470 && passed[0]) { nav = 12; setter(); }//play ashes to ashes  "2nd level"
					if (mousepos.y > 470 && mousepos.y < 720 && passed[1]) { nav = 13; setter(); }//play end of the line "3rd level"
				}
		}
		//...................How to Play...........
		if (nav == 21 || nav == 22 || nav == 23)
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
		//#################################################
		//hamed
		if (nav == 11)
		{
			gbg.setTexture(lvl1);
			if (s >= reqscore)
			{
				bound = 0;
				pass = 1;
				setter();
				levelup = 1;
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter) && s1.getPosition().x >= 1280)
			{
				bound = 1;
				s1.setPosition(20, 600);
				s = 0;
				nav = 12;
				levelup = 0;
			}
		}
		if (nav == 12) {
			gbg.setTexture(lvl2);
			reqscore = 250;
			if (s >= reqscore)
			{
				pass = 2;
				levelup = 1;
				bound = 0;
				setter();
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter) && s1.getPosition().x >= 1280)
			{
				bound = 1;
				s1.setPosition(20, 600);
				s = 0;
				levelup = 0;
				nav = 13;
			}
		}
		if (nav == 13)
		{
			gbg.setTexture(lvl3);
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

			}
		}
		//#################################################

		//.................game.....

		if (nav == 2 || nav == 11 || nav == 12 || nav == 13 || nav == 0 || nav == 1) {
			//hamed
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
				//#################################################

					//tebo
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
				//#################################################
			}
			//andrew
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
					dmgt += 10;
					if (shon)
					{
						shi -= 10;
						b -= 25;
					}
					else {
						h -= 10;
						a -= 25;
					}
					explosion.play();
				}
			}
			//nuke with tank
			for (int i = 0; i < 5; i++) {
				if (s1.getGlobalBounds().intersects(b2[i].getGlobalBounds())) {
					b2[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
					dmgt += 30;
					if (shon && shi < 30) {
						h -= 30 - shi;
						a -= 75 - b;
						shi = 0;
						b = 0;
					}
					else if (shon)
					{
						shi -= 30;
						b -= 75;
					}
					else {
						h -= 30;
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
				b += 125;
				if (b > 250)
					b = 250;
				shon = 1;
			}

			//bullet with nukes
			for (int i = 0; i < 5; i++) {
				if (s2.getGlobalBounds().intersects(b2[i].getGlobalBounds())) {
					//animation2[i].restart();
					exp2[i].setPosition(b2[i].getPosition().x - 100, b2[i].getPosition().y - 65);
					b2[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
					s += 10;
					fired = 0;
					s2.setPosition(800, 800);
					collided2[i] = 1;
					explosion.play();
					nukecnt++;
				}
			}
			//bullets with bombs
			for (int i = 0; i < 10; i++) {
				if (s2.getGlobalBounds().intersects(b1[i].getGlobalBounds())) {
					collided1[i] = 1;
					//animation1[i].restart();
					exp1[i].setPosition(b1[i].getPosition().x - 50, b1[i].getPosition().y - 30);
					b1[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
					s += 5;
					fired = 0;
					s2.setPosition(800, 800);
					explosion.play();
					bombcnt++;
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
			//#################################################

			//haitham
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
			//#################################################

		}
		if (nav == 0 || nav == 1) {
			gbg.setTexture(tg);
		}
		//.................survival..mode.......
		if (nav == 2) {
			gbg.setTexture(tg);
			//tebo
			//....repeat movement
			if (tb.getPosition().y > 720)//tank booster
				tb.setPosition(rand() % 1200, -7200);

			if (bb.getPosition().y > 720)//bullet booster
				bb.setPosition(rand() % 1200, -6480);
			//....move
			tb.move(0, 8); //tank booster
			bb.move(0, 8); //bullet booster
			//#################################################
			//andrew
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
			//#################################################

			//hamed
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
			//#################################################
			survcnt = survt.getElapsedTime().asSeconds();
		}
		//bebo
		//navigation after losing
		if (h <= 0) {
			if (nav == 2)
				nav = 32;
			if (nav == 11 || nav == 12 || nav == 13)
				nav = 31;
		}
		//#################################################

		if (shi <= 0)//andrew
			shon = 0;
		//bebo
		// ################ pause menu and go back###########
		if (Keyboard::isKeyPressed(Keyboard::Escape))
		{	//new added numbers
			if (nav == 1 || nav == 3 || nav == 4 || nav == 21 || nav == 22 || nav == 23)nav = 0;// go from campaign menu to main menu // new added credits and how to play and highscores
			if (nav == 2 || nav == 11 || nav == 12 || nav == 13)  pause = 1; //pause the game
			if (nav == 32 || nav == 31) { music.pause(); main(); } // play again

		}
		// new pause menu loop
		if (pause)
		{
			RenderWindow pausewindow(VideoMode(480, 240), "Pause Menu", Style::None);//^^^^^^^

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
					if (color)		pmt[0].setFillColor(Color::Red);
				}
				// change color of Main Menu text
				{
					if (mousepos1.x > 0 && mousepos1.x < 235 && mousepos1.y>190 && mousepos1.y < 227)
						color = 1;
					else 			color = 0;

					if (!color)		pmt[1].setFillColor(Color::White);
					if (color)		pmt[1].setFillColor(Color::Red);
				}
				//^change color of Options text ^^^^
				{
					if (mousepos1.x > 0 && mousepos1.x < 145 && mousepos1.y>100 && mousepos1.y < 138)
						color = 1;
					else			color = 0;
					if (!color)		pmt[2].setFillColor(Color::White);
					if (color)		pmt[2].setFillColor(Color::Red);

				}
				if (Mouse::isButtonPressed(Mouse::Left))//changed all conditions ^^^^^
				{
					if (mousepos1.x > 0 && mousepos1.x < 155 && mousepos1.y>11 && mousepos1.y < 50 && optionsB == 0)  //resume game
					{
						pause = 0;
						pausewindow.close();
					}
					if (mousepos1.x > 0 && mousepos1.x < 145 && mousepos1.y>100 && mousepos1.y < 138 && optionsB == 0)  //options
					{
						optionsB = 1;//^^^^^
					}
					if (mousepos1.x > 0 && mousepos1.x < 235 && mousepos1.y>190 && mousepos1.y < 227 && optionsB == 0)  //back to main menu
					{
						pause = 0;
						pausewindow.close();
						main();
					}
					if (mousepos1.x > 260 && mousepos1.x < 310 && mousepos1.y>10 && mousepos1.y < 60 && optionsB == 1)//^^^^
					{
						if (optmusic) optmusic = 0;
						if (!optmusic) optmusic = 1;
					}
					if (mousepos1.x > 260 && mousepos1.x < 310 && mousepos1.y>90 && mousepos1.y < 140 && optionsB == 1)//^^^^
					{
						if (optsound)optsound = 0;
						if (!optsound)optsound = 1;
					}

				}if (Keyboard::isKeyPressed(Keyboard::BackSpace))//^^^^
				{
					optionsB = 0;
				}


				pausewindow.clear();
				pausewindow.draw(PauseMenuBG);
				if (!optionsB)//^^^^
				{
					pausewindow.draw(pmt[0]);
					pausewindow.draw(pmt[1]);
					pausewindow.draw(pmt[2]);
				}
				if (optionsB)//^^^^
				{
					pausewindow.draw(options[0]);	pausewindow.draw(options[1]);	pausewindow.draw(options[2]);
					pausewindow.draw(BOX1);			pausewindow.draw(BOX2);
					if (optmusic) { pausewindow.draw(CHECK1); }
					if (optsound) { pausewindow.draw(CHECK2); }
				}
				pausewindow.display();


			}
		}
		//#################################################
		if (levelup) {
			if (arrowcnt.getElapsedTime().asSeconds() > 0.25) {
				zz++;
				zz %= 3;
				arrow.setTextureRect(IntRect(zz * 200, 0, 200, 188));
				arrowcnt.restart();

			}
		}
		//#########################
		dmg.setString("Damage taken: " + to_string(dmgt));
		nuken.setString("Number of nukes destroyed: " + to_string(nukecnt));
		bombn.setString("Number of bombs destroyed:" + to_string(bombcnt));


		//#################### draw ###################
		window.clear();
		//faisl
		//main menu
		window.draw(gbg);

		if (nav == 0) // main menu
		{

			for (int i = 3; i < 8; i++) // new changed numbers
			{
				window.draw(textCover[i]);
				window.draw(mmt[i]);
			}

		}
		if (nav == 1) // campaign menu
		{

			for (int i = 0; i < 3; i++)
			{
				window.draw(lvlbw[i]);
				window.draw(lvlcolored[i]);
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
		if (nav == 21 || nav == 22 || nav == 23)
		{
			window.draw(htpBG);
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
				for (int i = 0; i < 7; i++)
				{
					window.draw(htp23[i]);
				}
				for (int i = 3; i < 9; i++)
				{
					window.draw(HowtoPlay[i]);
				}
			}
		}
		//#################################################

		//####### game  #######
		if (nav == 2 || nav == 11 || nav == 12 || nav == 13 || nav == 0)
		{

			//hamed
			window.draw(s1);
			window.draw(s2);
			//#################################################

			if (bound) {
				//tebo
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
				
				//#################################################
				//andrew
				if (nav == 2 || nav == 11 || nav == 12 || nav == 13) {
					window.draw(r1);// sheild bar
					window.draw(r2);// health bar
					// text for score health sheild
					window.draw(score);	window.draw(health);
				}
				if (shon)
					window.draw(sheild);
				//#################################################
				//haitham
				//nuke explosion
				for (int i = 0; i < 5; i++) {
					window.draw(exp2[i]);
				}
				//bomb explosion
				for (int i = 0; i < 10; i++) {
					window.draw(exp1[i]);
				}
				//#################################################

			}
		}
		if (nav == 2)
		{	//boosters & boosters' icons
			window.draw(survtt);
			survtt.setString(to_string(survcnt / 60) + ":" + to_string(survcnt % 60));
			window.draw(tb); 	window.draw(bb);
			if (tbb)
				window.draw(tbi);
			if (bbb)
				window.draw(bbi);
			window.draw(ss);//sheild
		}
		//faisl
		//#######  ending  #######
		endgame[2].setString("Score: " + to_string(s));
		endgame[3].setString("Time: " + to_string(survcnt / 60) + ":" + to_string(survcnt % 60));
		if (nav == 31)
		{
			if (s >= 300)
			{
				window.draw(endgame[0]);
				window.draw(backToMain);
			}
			if (h <= 0) {
				window.draw(endgame[1]);
				window.draw(endgame[2]);
				window.draw(backToMain);
			}
		}
		if (nav == 32) {
			window.draw(endgame[2]);
			window.draw(endgame[3]);
			window.draw(backToMain);
		}
		if (levelup) {
			window.draw(arrow);
		}
		if (nav == 31 || nav == 32) {
			window.draw(dmg);
			window.draw(bombn);
			window.draw(nuken);
		}

		//#################################################

		window.display();
	}
	return 0;
}

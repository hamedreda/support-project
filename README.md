#include "SFML/Graphics.hpp";
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
	Clock animation1[10],animation2[5];
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
				if (mousepos.x > 45 && mousepos.x < 255 && mousepos.y>25 && mousepos.y < 76)
					color = 1;
				else 			color = 0;

				if (!color)		mmt[0].setFillColor(Color::White);
				if (color)		mmt[0].setFillColor(Color::Blue);
			}
			// change color of survival text
			{
				if (mousepos.x > 67 && mousepos.x < 237 && mousepos.y>125 && mousepos.y < 164)
					color = 1;
				else 			color = 0;

				if (!color)		mmt[1].setFillColor(Color::White);
				if (color)		mmt[1].setFillColor(Color::Blue);
			}
			// change color of exit text
			{
				if (mousepos.x > 100 && mousepos.x < 190 && mousepos.y>227 && mousepos.y < 268)
					color = 1;
				else            color = 0;

				if (!color)		mmt[2].setFillColor(Color::White);
				if (color)		mmt[2].setFillColor(Color::Blue);
			}
			//################# Navigation from main menu ##################
			if (Mouse::isButtonPressed(Mouse::Left))
			{
				if (mousepos.x > 45 && mousepos.x < 280 && mousepos.y > 25 && mousepos.y < 80)   nav = 1; // to campaign "click on campaign"
				if (mousepos.x > 67 && mousepos.x < 237 && mousepos.y>125 && mousepos.y < 164) {
					nav = 2; 
					survt.restart();
				}// to survival "click on survival"
				if (mousepos.x > 100 && mousepos.x < 190 && mousepos.y>227 && mousepos.y < 268) window.close(); //exit from main menu "click on exit"
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
		if (nav == 11) {
			if (s >= reqscore) {
				bound = 0;
				setter();
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter) && s1.getPosition().x >= 1280) {
				bound = 1;
				s1.setPosition(20, 600);
				s = 0;
				score.setString(to_string(s));
				nav = 12;
			}
		}
		if (nav == 12) {
			reqscore = 250;
			if (s >= reqscore) {
				bound = 0;
				setter();
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter) && s1.getPosition().x >= 1280) {
				bound = 1;
				s1.setPosition(20, 600);
				s = 0;
				score.setString(to_string(s));
				nav = 13;
			}
		}
		if (nav == 13) {
			reqscore = 300;
			if (s >= reqscore) {
				nav = 31;
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter) && s1.getPosition().x >= 1280) {
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

		//.................game..ending.....
		if (nav == 31 | nav ==32)
		{
			music.stop();
			if (Keyboard::isKeyPressed(Keyboard::Enter)) {
				main(); // TO  PLAY  AGAIN
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
		//main menu
		if (nav == 0)
		{
			window.draw(ibg);
			for (int i = 0; i < 3; i++)
			{
				window.draw(textCover[i]);
				window.draw(mmt[i]);
			}

		}
		// campaign menu
		if (nav == 1)
		{
			window.draw(ibg);
			for (int i = 3; i < 6; i++)
			{
				window.draw(textCover[i]);
				window.draw(mmt[i]);
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

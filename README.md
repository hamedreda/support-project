#include <SFML/Graphics.hpp>
using namespace std;
using namespace sf;
int scorec(int& x) {
	x += 300;
	return x;
}
int health1(int& x) {
	x -= 10;
	return x;
}
int health2(int& x) {
	x -= 30;
	return x;
}
int health3(int& x) {
	x += 10;
	return x;
}
int main()
{
	RenderWindow window(VideoMode(1280, 720), "Tanks"); //WINDOW
	RectangleShape r1(Vector2f(1280, 5)); r1.setPosition(0, 50); //WHITE LINE
	RectangleShape r2(Vector2f(250, 20)); r2.setPosition(1000, 15); r2.setFillColor(Color::Red); //HEALTH BAR
	window.setFramerateLimit(40); //fps
	Font f; f.loadFromFile("font.ttf"); //FONT
	int h = 100, s = 0,nav=0; //VARIABLES FOR SCORE , HEALTH & NAVIGATION

						// TEXT FOR SCORE & HEALTH :
	Text score, health;
	score.setFont(f);					health.setFont(f);
	score.setString(to_string(s));	    health.setString(to_string(h));
	score.setPosition(15, -5);		    health.setPosition(1100, -5);
	score.setFillColor(Color::Yellow);  health.setFillColor(Color::Yellow);
	score.setCharacterSize(50);		    health.setCharacterSize(50);
	
	//################################ Tank and bullets ###################################
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

	// TEXTURES FOR BOMBS & SHIELD
	Texture t1, t2, t3;
	t1.loadFromFile("bomb1.png"); 	t2.loadFromFile("bomb2.png"); 	t3.loadFromFile("shield.png");

	srand(time(0)); //for random positions 

	// spr arr FOR BOMBS& SHIELD
	Sprite b1[10]; //bomb1
	for (int i = 0; i < 10; i++)
	{
		b1[i].setTexture(t1);
		b1[i].setPosition(rand() % 1200, -rand() % 2000 * (rand() % 5 + 1));
		b1[i].setScale(0.09, 0.09);
	}
	Sprite b2[5]; //nuke
	for (int i = 0; i < 5; i++)
	{
		b2[i].setTexture(t2);
		b2[i].setPosition(rand() % 1200, -rand() % 2000 * (rand() % 5 + 1));
		b2[i].setScale(0.1, 0.1);
	}
	Sprite sh[3]; //shield
	for (int i = 0; i < 3; i++)
	{
		sh[i].setTexture(t3);
		sh[i].setPosition(rand() % 1200, -1440 * (i + 1));
		sh[i].setScale(0.03, 0.03);
	}
	bool fired = 0;
	//game ending txt
	Font ending;
	ending.loadFromFile("endgame.ttf");
	Text endgame[2];
	for (int i = 0; i < 2; i++) {
		endgame[i].setFont(ending);
		endgame[i].setCharacterSize(65);
		endgame[i].setPosition(300, 300);
		endgame[i].setFillColor(sf::Color::Red);
	}
	endgame[0].setString("Well done soldier");
	endgame[1].setString("Mission failed succesfully");

	while (window.isOpen())
	{

		// transporting through windows
		if (Keyboard::isKeyPressed(Keyboard::Space)) {
			nav = 1;
		}
		if (h == 0) {
			nav = 2;
		}
		if (s == 300) {
			nav = 2;
		}
		Event event; //to close window
		while (window.pollEvent(event))
		{
			if (event.type == Event::Closed)
				window.close();
		}
	    if(nav==1) {
		//######################## Movement ###############################
			//1-tank
		if (Keyboard::isKeyPressed(Keyboard::Right)) {
			s1.move(7, 0);
		}
		if (Keyboard::isKeyPressed(Keyboard::Left)) {
			s1.move(-7, 0);
		}
		//2-bullet
		if (Keyboard::isKeyPressed(Keyboard::Space) && fired == 0) {
			fired = 1;
			s2.setPosition(s1.getPosition().x + 78 / 2.0, 600);

		}
		//repeats bullets's movement
		if (fired) {
			s2.move(0, -10);
		}

		if (s2.getPosition().y < 0) {
			fired = 0;
			s2.setPosition(800, 800);
		}

		//repeat 
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
		for (int i = 0; i < 3; i++) //shield
		{
			if (sh[i].getPosition().y > 720)
				sh[i].setPosition(rand() % 1200, -3600);

		}
		//move
		for (int i = 0; i < 10; i++) // bomb1
		{
			b1[i].move(0, 8);
		}
		for (int i = 0; i < 5; i++)  //nuke
		{
			b2[i].move(0, 8);
		}
		for (int i = 0; i < 3; i++) //shield
		{
			sh[i].move(0, 8);
		}
		// #############################collision######################
		//bombs with the tank
		for (int i = 0; i < 10; i++) {
			if (s1.getGlobalBounds().intersects(b1[i].getGlobalBounds())) {
				b1[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
				h = health1(h);
				health.setString(to_string(h));

			}
		}
		//nuke with tank
		for (int i = 0; i < 5; i++) {
			if (s1.getGlobalBounds().intersects(b2[i].getGlobalBounds())) {
				b2[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
				h = health2(h);
				health.setString(to_string(h));

			}
		}
		//shield with the tank
		for (int i = 0; i < 3; i++) {
			if (s1.getGlobalBounds().intersects(sh[i].getGlobalBounds())) {
				sh[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
				h = health3(h);
				health.setString(to_string(h));

			}
		}
		//bullet with nukes
		for (int i = 0; i < 5; i++) {
			if (s2.getGlobalBounds().intersects(b2[i].getGlobalBounds())) {
				b2[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
				s = scorec(s);
				score.setString(to_string(s));
				fired = 0;
				s2.setPosition(800, 800);

			}
		}
		//bullets with bombs
		for (int i = 0; i < 10; i++) {
			if (s2.getGlobalBounds().intersects(b1[i].getGlobalBounds())) {
				b1[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
				s = scorec(s);
				score.setString(to_string(s));
				fired = 0;
				s2.setPosition(800, 800);
			}
		}
	}
		//drawing :
		window.clear();
		if (nav == 1) {
			window.draw(s1);
			window.draw(s2);
			for (int i = 0; i < 10; i++) //bomb 1
			{
				window.draw(b1[i]);
			}
			for (int i = 0; i < 5; i++) //bomb 2
			{
				window.draw(b2[i]);
			}
			for (int i = 0; i < 3; i++) //shield
			{
				window.draw(sh[i]);
			}
			window.draw(r1);	window.draw(r2);
			window.draw(score);	window.draw(health);
		}
		if (nav == 2) {
			if (s == 300) {
				window.draw(endgame[0]);
			}
			if (h == 0) {
				window.draw(endgame[1]);
			}
		}
		window.display();
	}

	return 0;
}

#include <SFML/Graphics.hpp>
#include <SFML/audio.hpp>
using namespace sf;
int main()
{
	RenderWindow window(sf::VideoMode(1280, 720), "RUN FOR LIFE");
	Event event; //to close windows
	window.setFramerateLimit(40);

	//################################ Tank and bullets ###################################
	//1-tank
	Texture t;
	t.loadFromFile("Tank.png");
	Sprite s(t);
	s.setTextureRect(IntRect(0, 0, 476, 524));
	s.setScale(0.2, 0.2);
	s.setPosition(20, 600);
	//2-bullets
	Texture t2;
	t2.loadFromFile("bullet.png");
	Sprite s2(t2);
	s2.setTextureRect(IntRect(0, 0, 368, 677));
	s2.setScale(0.025, 0.025);
	s2.setPosition(800, 720);
	bool fired = 0;
	while (window.isOpen()) {
		//######################## Movement ###############################
			//1-tank
		if (Keyboard::isKeyPressed(Keyboard::Right)) {
			s.move(7, 0);
		}
		if (Keyboard::isKeyPressed(Keyboard::Left)) {
			s.move(-7, 0);
		}
		//2-bullet
		if (Keyboard::isKeyPressed(Keyboard::Space)&&fired==0) {
			fired = 1;
			s2.setPosition(s.getPosition().x + 78 / 2.0, 600);

		}
		//repeats bullets's movement
		if (fired) {
			s2.move(0, -10);
		}

		if (s2.getPosition().y <0) {
			fired = 0;
			s2.setPosition(800, 800);
		}
		while (window.pollEvent(event))
		{

			if (event.type == Event::Closed)
				window.close();
		}
		window.clear();
		window.draw(s);
		window.draw(s2);
		window.display();
	}
	return 0;
}

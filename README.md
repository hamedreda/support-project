#include <SFML/Graphics.hpp>
#include <SFML/audio.hpp>
#include<iostream>
using namespace std;
using namespace sf;
int main()
{
    RenderWindow window(sf::VideoMode(1280, 720), "SFML works!");
    //music
    Music music;    // audio
    music.setLoop("Game audio1.ogg");
    music.openFromFile("Game audio1.ogg");
    music.play();
     int nav = 0; // navigation counter
     Texture b; // menu intero background
     b.loadFromFile("intero war.jpg");
     Sprite backg(b);
     backg.setScale(0.5, 0.5);

     Texture t; // menu  background
     t.loadFromFile("plan.jpg");
     Sprite plan(t);
     plan.setScale(5.1, 3.7);
     Texture p; // menu button background
     p.loadFromFile("boom.jpg");
     Sprite boomb(p);
     boomb.setScale(0.1, 0.1);
     boomb.setPosition(70, 80);
     Texture g; //  background nav 3
     g.loadFromFile("back grond.jpg");
     Sprite background(g);
     background.setScale(0.699, 0.9);
     
    
    while (window.isOpen())
    {
        if (Keyboard::isKeyPressed(Keyboard::Space)) {  //menu
            nav = 1;
        }
        if (Keyboard::isKeyPressed(Keyboard::Escape)) {//
            nav = 0;
        }
        // mouse 
        Mouse ms;
        //menu navigating
        if (Mouse::isButtonPressed(Mouse::Left)) {
            if (ms.getPosition(window).x > 50 && ms.getPosition(window).x < 380 &&
                ms.getPosition(window).y>40 && ms.getPosition(window).y < 270) {
                nav = 3;
            }
           
        }
       
        sf::Event event;
        
        while (window.pollEvent(event))
        {
            

            if (event.type == Event::Closed)
                window.close();

            

        }

            window.clear();
            if (nav == 0)
               window.draw(backg);

            if (nav == 1) {
                window.draw(plan);
                window.draw(boomb);
            }
               // music.play();
            if (nav == 3) {
                window.draw(background);
                
            }
            window.display();
        
    }
    return 0;
}
    

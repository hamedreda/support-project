//counter for boundries 
	int bound = 0;
	//speed increser
	int speed = 0;






//#####################BOUNDRIES############################
			//leftwall
			if (s1.getPosition().x <= 0 && Keyboard::isKeyPressed(Keyboard::Left)) {
				s1.move(7, 0);
			}
			//rightwall
			if (s1.getPosition().x >= 1184.8 && Keyboard::isKeyPressed(Keyboard::Right) && s<200 ) {
				s1.move(-7, 0);
			}
			if (s >= 200 && s1.getPosition().x > 1280 && bound==0) {
				s1.setPosition(20, 600);
				bound++;
				speed += 2;
			}
			if (s1.getPosition().x >= 1184.8 && Keyboard::isKeyPressed(Keyboard::Right) && s > 200 && bound==1) {
				s1.move(-7, 0);
			}
//#####################MOVEMENT#############################
for (int i = 0; i < 10; i++) // bomb1
			{
				if (s < 200 || (s>=200 && bound==1 )) {
					b1[i].move(0, 8+speed);	
				}
				else {
					if (b1[i].getPosition().y > -72 && b1[i].getPosition().y < 720) {
						b1[i].move(0, 8+speed);
					}
				}
			}
			for (int i = 0; i < 5; i++)  //nuke
			{
				if (s < 200 || (s >= 200 && bound == 1)) {
					b2[i].move(0, 8+speed);
				}
				else {
					if (b2[i].getPosition().y > -64.1 && b2[i].getPosition().y < 720) {
						b2[i].move(0, 8+speed);
					}
				}
			}
			for (int i = 0; i < 3; i++) //shield
			{
				if (s < 200 || (s >= 200 && bound == 1)) {
					sh[i].move(0, 8+speed);
				}
				else {
					if (sh[i].getPosition().y < 720 && sh[i].getPosition().y > -69.3) {
						sh[i].move(0, 8+speed);
					}
				}
			}

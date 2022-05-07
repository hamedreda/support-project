//survival time&speed
Clock survt;
int reqtime = 25;
int speed = 0;




if (mousepos.x > 67 && mousepos.x < 237 && mousepos.y>125 && mousepos.y < 164) {
				nav = 2;
				survt.restart();
			} // to survival "click on survival"
			
			
			
			
	//move
			for (int i = 0; i < 10; i++) // bomb1
			{
				b1[i].move(0, 8+speed);
			}
			for (int i = 0; i < 5; i++)  //nuke
			{
				b2[i].move(0, 8+speed);
			}
			
			//.................survival..mode.......
	if (nav == 2) {
		Time survcnt = survt.getElapsedTime();
		if (survcnt.asSeconds() > reqtime) {
			speed += 2;
			reqtime += 25;
		}

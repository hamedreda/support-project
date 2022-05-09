clock animation1[10],animation2[5
bool collided1[10]={0}, collided2[5]={0
texture exp;
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
	//دي حاجات مكنتش موجودة هتزودها#########################################################################################
	//bullet with nukes
			for (int i = 0; i < 5; i++) {
				if (s2.getGlobalBounds().intersects(b2[i].getGlobalBounds())) {
					animation2[i].restart();
					exp2[i].setPosition(b2[i].getPosition().x-100, b2[i].getPosition().y-65 );
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
					exp1[i].setPosition(b1[i].getPosition().x-50, b1[i].getPosition().y-30);
					b1[i].setPosition(rand() % 1200, -rand() % 720 * (rand() % 5 + 1));
					s += 10;
					score.setString(to_string(s));
					fired = 0;
					s2.setPosition(800, 800);
					explosion.play();
				}
			}
		
		//دول موجودين كده كده بس انا زودت فيهم حاجات فخدهم و امسح القدام############################################################################
		//explosion animation
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
			exp2[i].setTextureRect(IntRect(cnt1x[i] * 145,cnt1y[i]*150, 145, 150));
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
		//ده الجزء بتاع الانميشن#######################################################################################
		for (int i = 0; i < 5; i++) {//nuke explosion
				window.draw(exp2[i]);
			}
			for (int i = 0; i < 10; i++) {//bomb explosion
				window.draw(exp1[i]);
			}
			//ده الدرو بتاع الاكسبلوجن##############################################################################

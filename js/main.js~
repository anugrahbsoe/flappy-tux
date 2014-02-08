// Start enchant.js
enchant();
 
window.onload = function() {
    // Starting point
    var game = new Game(320, 568);
    game.preload('res/bg.png',
					'res/fish1.png',
					'res/pipe.png',
					'res/floor.png');
    game.fps = 30;
    game.scale = 1;
    game.onload = function() {
        // Once Game finish loading
        console.log("Hellow, World!");
		var scene = new GameScene();
		game.pushScene(scene);
    }
    window.scrollTo(0,0);
    game.start();  
	 
};

var GameScene = Class.create(Scene, {
	
	initialize: function() {
		var game, bg, physicsWorld;
		
		game = Game.instance;
		Scene.apply(this);
		
		physicsWorld = new PhysicsWorld(0, 9.8);
		
		bg = new Sprite(320, 568);
		bg.image = game.assets['res/bg.png'];
		this.addChild(bg);
		
		pipeGroup = new Group();
		this.addChild(pipeGroup);
		
		floor = new Sprite(320,80);
		floor.image = game.assets['res/floor.png'];
		floor.y = game.height - 80
		this.addChild(floor);
		
		player = new Fish();
		this.addChild(player);
		
		var timeElapsed = 0
		var jumped = false
		
		var canGeneratePipe = true
		
		this.addEventListener("enterframe", function() {
			
			if (canGeneratePipe) {
				this.generatePipe()
				canGeneratePipe = false
				this.tl.delay(50).then(function() {canGeneratePipe = true});
			}
			
			if (jumped == false) {
				player.y+=15;
			}
			
			if (player.y >= game.height || player.intersect(floor)) {
				console.log("Game over")
				game.stop()
			}
			
		});
		
		this.addEventListener("touchstart", function() {
			if (jumped == false && player.y > 0) {
				jumped = true
				player.tl.moveBy(0,-75,10,enchant.Easing.QUINT_EASEOUT).exec(function() {
					jumped = false;
				});
				timeElapsed = 1;
			} else if (jumped == true && player.y > 0) {
				player.tl.clear();
				player.tl.moveBy(0,-75,10,enchant.Easing.QUINT_EASEOUT).exec(function() {
					jumped = false;
				});
				timeElapsed = 1;
			}
		});
	
	},
	
	generatePipe: function() {
		
		var game = Game.instance;
		
		pipePosY = Math.floor(Math.random()*150)+100;
		console.log(pipePosY)
		
		var topPipe = new Pipe();
		var bottomPipe = new PipeBottom();
		pipeGroup.addChild(topPipe);
		pipeGroup.addChild(bottomPipe);
		
		topPipe.x = 350;
		bottomPipe.x = 350;
		
		topPipe.y = pipePosY - 376;
		bottomPipe.y = pipePosY + 150;
		
		topPipe.tl.moveX(-100, 80)
		bottomPipe.tl.moveX(-100, 80)
		
		this.addEventListener("enterframe", function() {
			if (topPipe.intersect(player) || bottomPipe.intersect(player)) {
				console.log("Game over cuz Pipe")
				game.stop()
			}
		});
		
		
	}
	
});

var Fish = Class.create(Sprite, {
	
	initialize: function() {
		var game;
		
		game = Game.instance;
		Sprite.apply(this,[64,36]);
		this.image = game.assets['res/fish1.png'];
		this.x = 50;
		this.y = 100;
	}
	
});

var Pipe = Class.create(Sprite, {
		
	initialize: function() {
		var game;
		
		game = Game.instance;
		Sprite.apply(this,[88,376]);
		this.image = game.assets['res/pipe.png'];
		this.x = 200;
		this.y = 0;
		
	}
	
});

var PipeBottom = Class.create(Sprite, {
		
	initialize: function() {
		var game;
		
		game = Game.instance;
		Sprite.apply(this,[88,376]);
		this.image = game.assets['res/pipe.png'];
		this.x = 200;
		this.y = 0;
		this.scaleY = -1;
	}
	
});
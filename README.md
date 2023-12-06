<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EcoExplorer: The Environmental Adventure Game</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background-color: #87CEEB; /* Light Blue */
        }

        #game-container {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
    </style>
</head>
<body>
    <div id="game-container"></div>

    <script src="https://cdn.jsdelivr.net/npm/phaser@3.55.2/dist/phaser.min.js"></script>
    <script>
        // Phaser Game Configuration
        const config = {
            type: Phaser.AUTO,
            width: 800,
            height: 600,
            physics: {
                default: 'arcade',
                arcade: {
                    gravity: { y: 300 },
                    debug: false
                }
            },
            scene: {
                preload: preload,
                create: create,
                update: update
            }
        };

        // Create a new Phaser.Game instance
        const game = new Phaser.Game(config);

        function preload() {
            this.load.image('sky', 'assets/sky.png');
            this.load.spritesheet('dude', 'assets/dude.png', { frameWidth: 32, frameHeight: 48 });
        }

        function create() {
            this.add.image(400, 300, 'sky');

            player = this.physics.add.sprite(100, 450, 'dude');
            player.setBounce(0.2);
            player.setCollideWorldBounds(true);

            this.physics.add.collider(player, platforms);

            this.anims.create({
                key: 'left',
                frames: this.anims.generateFrameNumbers('dude', { start: 0, end: 3 }),
                frameRate: 10,
                repeat: -1
            });

            this.anims.create({
                key: 'turn',
                frames: [ { key: 'dude', frame: 4 } ],
                frameRate: 20
            });

            this.anims.create({
                key: 'right',
                frames: this.anims.generateFrameNumbers('dude', { start: 5, end: 8 }),
                frameRate: 10,
                repeat: -1
            });
        }

        function update() {
            cursors = this.input.keyboard.createCursorKeys();

            if (cursors.left.isDown) {
                player.setVelocityX(-160);
                player.anims.play('left', true);
            }
            else if (cursors.right.isDown) {
                player.setVelocityX(160);
                player.anims.play('right', true);
            }
            else {
                player.setVelocityX(0);
                player.anims.play('turn');
            }

            if (cursors.up.isDown && player.body.touching.down) {
                player.setVelocityY(-330);
            }
        }
    </script>
</body>
</html>

title: To infinity and beyond
author: strahius
categories:
  - Ad Astra
tags:
  - Phaser CE
  - Scrolling
  - Texture
date: 2017-05-01 20:37:00
---
Space shooters can deal with space movement in various ways. Asteroids has the player's position being flipped on the other side, when touching the edge of the screen, {% link Geometry Wars https://en.wikipedia.org/wiki/Geometry_Wars:_Retro_Evolved Geometry Wars: Retro Evolved %} restricts the movement in a confined box, etc. Since space is infinite, I thought it would be cool to implement that "feature" in the game as well. 

Phaser CE does not provide this out of the box, but it does provide the tools to do it. We start with a background that looks like this:

{% asset_img stars.png Stars background %}

It's just a bunch of white spots on a transparent canvas (I added some color in the background to make them visible on the blog).

{% codeblock lang:js %}
this.game.stage.backgroundColor = '#222523';
this.background = new Phaser.TileSprite(this.game,
                                        0 - (this.game.world.centerX / 2),
                                        0 - (this.game.world.centerY / 2),
                                        BG_SIZE, BG_SIZE,
                                        'stars');
this.background.fixedToCamera = true;
{% endcodeblock %}

Initially we load our background in _create_. We use a tile sprite, since we'll be be "rolling" the texture on the sprite. We also set it to be fixed to the camera, as it won't be automatically moving according to the camera movement. It will always stay at the original position.

Then in _update_, we need to do two things. Firstly, we move the tile's position in the oposite way the camera is moving (this feels a bit like Futurama's {% link dark matter engine http://futurama.wikia.com/wiki/Dark_matter_engine Dark matter engine %}). Secondly, we set the new camera bounds around the player.

{% codeblock lang:js %}
this.background.tilePosition.x = -this.game.camera.x;
this.background.tilePosition.y = -this.game.camera.y;

this.game.world.bounds.centerOn(this.player.body.x, this.player.body.y);
this.game.camera.setBoundsToWorld();
{% endcodeblock %}

If I had to make an analogy, imagine space being a toilet paper roll. And on the paper, there are the stars that you want to show.

{% asset_img space_toilet_paper.png Space toiler paper %}

The roll stays always on view, fixed to the same position as our player moves. The paper is rolling to the left or right, according to where the player is going, and creates the illusion of movement.

And here's our toilet paper rolling stars background:

{% asset_img infinite_background.gif Infinite background %}
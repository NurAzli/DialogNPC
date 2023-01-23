# NpcDialog

### Download

The compiled phar is available [on poggit](https://poggit.pmmp.io/ci/GiantQuartz/NpcDialog/NpcDialog)

### How to use

First, you have to register the virion, you can do this during the `onEnable()` of your plugin main class.

```php
public function onEnable(): void {
    NpcDialog::register($this);
}
```

Then you have to spawn or get the object of the entity you want to have the dialog form (it can't be a player!). For
this example, I will spawn a zombie using PocketMine built-in methods.

 ```php
$nbt = Entity::createBaseNBT($player, null, $player->yaw, $player->pitch);
$entity = Entity::createEntity("Zombie", $player->level, $nbt);
$entity->spawnToAll();
 
$entity->setNameTag("Jerry The Zombie!");
 ```

Finally you will have to create the form and pair it with the entity.

```php
$form = new DialogForm("This is the dialog text");
 
$form->addButton("Hi", function(Player $player) {
    $player->sendMessage("Hi!!");
});

$form->setCloseListener(function(Player $player) {
    $player->sendMessage("You closed the form!");
});

$form->pairWithEntity($entity);
```

This can be trimmed down to a single line:

```php
(new DialogForm("This is the dialog text", function(Player $player){ $player->sendMessage("You closed the form!"); }))->addButton("Hi", function(Player $player){ $player->sendMessage("Hi!!"); })->pairWithEntity($entity);
```

The result of this example would be an entity showing this when it's right-clicked (or hold in the mobile versions):

![Example](https://i.imgur.com/468mQKF.png)

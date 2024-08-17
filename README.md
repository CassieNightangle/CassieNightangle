from panda3d.core import *
from direct.showbase.ShowBase import ShowBase
from direct.gui.OnscreenText import OnscreenText
from direct.gui.DirectGui import DirectButton, DirectFrame
import sys

# Game data
characters = [
    {'name': 'Nadaoka', 'element': 'Fire', 'model': 'models/nadaoka.egg'},
    {'name': 'Kurono', 'element': 'Water', 'model': 'models/kurono.egg'},
    {'name': 'Seon', 'element': 'Ice', 'model': 'models/seon.egg'},
    {'name': 'Tayong', 'element': 'Wind', 'model': 'models/tayong.egg'},
    {'name': 'Ryoto', 'element': 'Grass', 'model': 'models/ryoto.egg'},
    {'name': 'Hishiro', 'element': 'Electric', 'model': 'models/hishiro.egg'},
    {'name': 'Tairi', 'element': 'Fire', 'model': 'models/tairi.egg'},
    {'name': 'Naomi', 'element': 'Water', 'model': 'models/naomi.egg'},
    {'name': 'Hana', 'element': 'Ice', 'model': 'models/hana.egg'},
    {'name': 'Kohima', 'element': 'Wind', 'model': 'models/kohima.egg'},
    {'name': 'Suzuki', 'element': 'Grass', 'model': 'models/suzuki.egg'},
    {'name': 'Filavandrel', 'element': 'Electric', 'model': 'models/filavandrel.egg'}
]

maps = [
    {'name': 'The Fume', 'model': 'models/the_fume.egg'},
    {'name': 'Callisto', 'model': 'models/callisto.egg'},
    {'name': 'Elendil', 'model': 'models/elendil.egg'},
    {'name': 'Caspion', 'model': 'models/caspion.egg'},
    {'name': 'Aragon', 'model': 'models/aragon.egg'},
    {'name': 'Filavandrel', 'model': 'models/filavandrel_map.egg'},
    {'name': 'The Crimson', 'model': 'models/the_crimson.egg'},
    {'name': 'The Land of Wave', 'model': 'models/the_land_of_wave.egg'},
    {'name': 'Thunderland', 'model': 'models/thunderland.egg'},
    {'name': 'Ever Dark', 'model': 'models/ever_dark.egg'},
    {'name': 'The Gale', 'model': 'models/the_gale.egg'},
    {'name': 'The Frost', 'model': 'models/the_frost.egg'}
]

class Game(ShowBase):
    def __init__(self):
        ShowBase.__init__(self)

        # Background setup
        self.set_background()

        # Character selection frame
        self.character_frame = DirectFrame(frameColor=(0, 0, 0, 0), pos=(0, 0, 0.8))
        self.selected_character = None
        self.selected_map = None

        # Create buttons for character selection
        self.create_character_buttons()

        # Map selection frame
        self.map_frame = DirectFrame(frameColor=(0, 0, 0, 0), pos=(0, 0, 0.5))
        self.create_map_buttons()

        # Start game button
        self.start_button = DirectButton(text="Start Battle", scale=0.1, command=self.start_game, pos=(0, 0, 0.3))

    def set_background(self):
        self.set_background_color(0.1, 0.1, 0.1)  # Dark gray background

    def create_character_buttons(self):
        for idx, char in enumerate(characters):
            button = DirectButton(text=char['name'],
                                  scale=0.1,
                                  command=lambda c=char: self.select_character(c),
                                  pos=(-0.7 + (idx % 3) * 0.5, 0, 0.9 - (idx // 3) * 0.1))
            button.setTransparency(TransparencyAttrib.MAlpha)

    def create_map_buttons(self):
        for idx, map in enumerate(maps):
            button = DirectButton(text=map['name'],
                                  scale=0.1,
                                  command=lambda m=map: self.select_map(m),
                                  pos=(-0.7 + (idx % 3) * 0.5, 0, 0.6 - (idx // 3) * 0.1))
            button.setTransparency(TransparencyAttrib.MAlpha)

    def select_character(self, character):
        self.selected_character = character
        print(f"Selected Character: {character['name']}")
        self.display_selected_character()

    def select_map(self, map):
        self.selected_map = map
        print(f"Selected Map: {map['name']}")
        self.display_selected_map()

    def display_selected_character(self):
        OnscreenText(text=f"Selected Character: {self.selected_character['name']}",
                      pos=(-0.5, 0.9), scale=0.07, fg=(1, 1, 1, 1))

    def display_selected_map(self):
        OnscreenText(text=f"Selected Map: {self.selected_map['name']}",
                      pos=(-0.5, 0.8), scale=0.07, fg=(1, 1, 1, 1))

    def start_game(self):
        if self.selected_character and self.selected_map:
            self.load_battle_scene()
        else:
            print("Please select both a character and a map.")

    def load_battle_scene(self):
        self.character_frame.hide()
        self.map_frame.hide()
        self.start_button.hide()

        # Load character model
        char_model = self.loader.loadModel(self.selected_character['model'])
        char_model.reparentTo(self.render)
        char_model.setScale(0.1, 0.1, 0.1)
        char_model.setPos(0, 10, 0)

        # Load map model
        map_model = self.loader.loadModel(self.selected_map['model'])
        map_model.reparentTo(self.render)
        map_model.setScale(0.5, 0.5, 0.5)
        map_model.setPos(0, 0, -1)

        # Start battle animation or logic here
        print(f"Starting battle with {self.selected_character['name']} on {self.selected_map['name']}")

# Start the game
if __name__ == "__main__":
    game = Game()
    game.run()


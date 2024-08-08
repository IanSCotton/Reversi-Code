# Overview of the Reversi code:
The overall function of the code is to allow the player to play against or watch different AIs in Reversi.

## Functions of the code:
### Pre-game:
•	The player can choose from a wide array of AIs and themselves who will play as purple (going first) and who will play as yellow (going second), each AI has specific stats and descriptions. \n
•	The player can use a slider to decide on the lengths of animations from 0-100 (milliseconds)
•	The player can click on a button that shows how to play, it has three panels simply explaining the game
•	The player can click on a button that shows all AIs, once pressed this cannot be unpressed and will allow the player to choose a wider array of AIs to play against
•	The player can start the game

### In game:
•	The code allows people to play games of Reversi, inside games players do this by clicking on white tiles when it is their turn, the effect of the moves and the legal moves are calculated by the programme.
•	When a move is played the tile played on temporarily turns black and the effected tiles change to the appropriate colour following a sequence of intermediate colours.
•	The programme works out when the game has ended and congratulates the winning player.

### Post-game:
•	The code shows the winning side and the number of tiles each side has
•	The player has the choice of either “Play Again” (with the same settings) or going back to the pre-game screen with “New Game” 

## Overview of the layout:
# Sections: (all line numbers and lengths are subject to change)
•	Initial setup including imports (lines 1-123)
•	Finding AI moves and the AIs choosing moves (lines 126-868)
•	Making moves and updating the board (lines 872-1012)
•	Printing the board (lines 1015-1170)
•	Creating the start game menu (lines 1173-1606) 


### Global variables: any variables that are used with global whilst in functions as they may need to be updated from inside a function.
•	grid (this variable holds the grid, and each tile can be accessed with grid[row][column]
•	grid_last (this variable holds the previous grid, used for comparison, and can be accessed the same way)
•	turn (this variable shows the current turn, it is either 0 or 1)
•	ai_turn (this variable show whether the specific turn (turn 0 or 1) is to be played by a human or AI)
•	default_animation_timer (this shows the value that the animation slider is set to when the start-screen is created)
•	initial_settings (this shows the value of the initial settings for purple and yellow in the start-screen)
•	options (this shows all of the different players and AIs that can be selected)
•	how_to_play_flag (this flag checks whether the how_to_play box is currently being shows or not)
•	show_play_frame (this is the frame that holds the how_to_play box)
•	game_frame_list (this holds the game_frames that the game is displayed in)
•	window_frame_frame (this holds the start of game screen)
•	results (this shows what the player has picked for who plays purple and orange)
•	animations_slider (this holds the animation slider which can be used to determine what the player has chosen the animation length to be)
 
## Overview of initial setup (lines 1-123)
•	import useful packages
•	set default options for the animation-timer, the selected AIs and the depth that certain AIs will look to.
•	Create the options and longer all_options for the drop-down menus when selecting the AIs
•	Create the descriptions for the options and their scores.
•	Create the grid, the grid_last and choose the board_size
•	If needed have a way of setting a specific position and swapping the two tile colours across the entire board.
•	Create arrays used to check all of the possible directions around a cell and the colours
•	Set the turn
•	Create function to swap the turn

# Functions:
•	the_swap(a_grid): this goes through the entire grid and swaps purple and yellow tiles, it returns a modified version of a_grid
•	swap_turn(binary_variable): this takes a 0 or 1 and returns the opposite e.g. 0 returns 1,0 and 1 returns 0, 1
 
### Overview of finding AI moves and the AIs choosing moves (lines 126-868)

### Functions and classes:
•	move_list_finder(grid) notes all of the legal moves in a position returns move_list
•	first_move(move_list) returns the first legal move
•	random_move(move_list) returns a random move
•	border_control(move_list)  plays a move that tries to control the border returns list of best moves
•	control_tile_finder(I, j, protected_grid, take, grid) uses protected_grid to determine whether the specific tile(i,j) can be captured on take’s turn or can be played on returns updated protected_grid.
•	Control_grid_finder(grid) finds all the tiles on the board that can no longer be captured by the opponent using control_tile_finder returns a protected_grid
•	border_control _test_2 (move_list) tries to play on safe tiles as decided by control_grid_finder returns list of best moves
•	opponent_min_or_max(move_list, max_or_min) plays the move that either maximises or minimises the number of tiles the current player has returns the first move that maximises or minimises
•	create_tree(grid, layers) part of my method to solve a tree to find the best move, this creates a tree of all positions to a specific layer, each new layer is stored at a higher number in the array it returns the game_tree 
•	fill_bottom_tree_maximiser(game_tree, layers) go to the bottom of the game_tree and set a score for each board based on the number of each tile returns the modified game_tree
•	solve_tree(game_tree, layers) solves the tree using a min-max algorithm to find the best move in the initial position returns the best move
•	fill_bottom_tree_border(game_tree, layers) fill the bottom of the game_tree using the border_control method to assign points to each board returns the game tree.
•	future_border_test(layers): uses create_tree, fill_bottom_tree_border and solve_tree to create fill and solve the tree to return the best move
•	fill_bottom_tree_border_2(game_tree, layers) fills the bottom of the game_tree with scores based on the protected_move_grids and the number of tiles returns game_tree
•	future_border_test(layers): uses create_tree, fill_bottom_tree_border_2 and solve_tree to create fill and solve the tree to return the best move
•	find_score_board_maximiser(grid) this returns a score for a grid based on the ratio between the number of black and white tiles returns the score
•	find_score_border(grid, protected_move_grid) uses the protected_move_grid, the number of tiles of each colour, a list of bad and very bad tiles to return a score for the board
•	class EvaluationGrids(grid, current_depth, turn, target_depth, setting): this class contains data for all of the inputted variables as well as the associated protected_move_grid, any grids directly formed from playing moves on the grid and an evaluation of the current position if current_depth=target_depth.
•	Sort_moves(evaluation_grid): takes an evaluation_grid and orders its moves from best to worst based on how good they are likely to be, starting with moves that would make tiles uncapturable and then based on the number of tiles they take over, returns the under_grid_grids based on playing moves on the under_grids and the sorted_move list
•	Evaluation_functions(setting, evaluation_grid): decides whether the score should be based on the maximising or border method and uses find_score_board_maximiser or find_score_border to return the score as required
•	Min_max(evaluation_grid, max_depth, alpha, beta): uses a min_max algorithm with alpha beta pruning with recursion to fill out the evaluation_grid evaluations to help the best move returns an evaluation_grid with more evaluations
•	Find_move(evaluation_grid) after all the layers have been evaluated this returns the first move in the list that has an evaluation equal to the overall evaluation of the current position (the best move)
•	Cheater_code(move_list): This is complex enough to have its own subsection below, it either plays a random move returning the move or directly modifies the board with one of the functions below returning a pointless move [-2 ,-2] 
•	all_placer(): this places a tile on every square where a legal move could be played returns the modified grid
•	place_square(): this places a square of size 2-4 somewhere on the board returns the modified grid
•	flip_straight(): this takes a random row or column and flips all of the tiles in it returns the modified grid
•	smiley_face(grid, turn): this is sometimes played at the start of the game and displays a specific pattern on the board returns the modified grid
•	ai_player(setting): unless there’s cheating if there is only one move return that move otherwise return a move using the strategy of one of these AIs:

### AI list:
Player: controlled by the player
1st_move: plays the 1st move in the move list
Random: plays random moves
border_control: Compares moves to a grid of move quality and plays the best one
Tile_Maximiser: Plays the move that minimises the opponent’s tiles
Border_Controller: Uses the protected move grid to try to play slightly better moves then plays border_control
opponent_maximiser: Maximises the number of tiles the opponent has
border_control_maximiser: looks at the border control grid first and plays the move that maximises its tiles whilst scoring highest
future_maximiser_test: Looks into the future using the AI_tree_layers approach to maximise its tiles
future_border_test: : Looks into the future using the AI_tree_layers approach to maximise its score, score based off border_control
future_border_test_2:  : Looks into the future using the AI_tree_layers approach to maximise its score, score based off border_control_2
Oracle_Maximiser: Looks into the future using the class and a,b pruning approach to maximise its tiles
Oracle_border: Looks into the future using the class and a,b pruning approach to maximise its score, using a mixture of maximisation and protected_move_grid
 
## Making moves and updating the board (lines 872-1012)
•	Define the overall play_move function which manages the rest of this section, it runs updater, checks for legal moves, if there are none it runs updater (this time for the other player) it then runs printer updating the board 
•	Updater gets a copy of the current board to get differences for printing later, if gets the move depending on which AI is active (or the player move if the player is playing) it plays the move and updates nearby cells (grid_changer), swaps the turn (swap_turn), finds all of the legal moves (legal_moves), finds the differences between the current and old position (difference_finder). 


### Functions:
•	play_move(i, j, legal_moves_check, animation_timer):  runs updater, checks for legal moves, if there are none it runs updater(i, j, legal_moves_check) (this time for the other player) it then runs printer updating the board never returns
•	grid_changer(i, j, grid, turn): updates the cell and all cells around it returns the new grid
•	legal_moves_of_cell(grid, turn, i, j) finds whether a specific cell (i,j) has any legal moves, if so it sets it to “v”, returns the new grid
•	legal_moves(grid, turn): uses legal_moves_of_cell to find all of the legal moves of the board returns the new grid
•	difference_finder(grid, last_grid) compares the current and former grid to find any differences, counts the tiles and checks for legal moves returns the number of white tiles, the number of black tiles, the differences(in terms of i) the differences(in terms of j) the new tiles they changes to and whether there are legal moves
•	updater(i, j, legal_moves_check): creates a copy of the current board, gets the move as necessary with ai_player, updates the grid with grid_changer, swaps the turn with swap_turn, finds the legal moves of the grid with legal_moves, finds the differences between the new and old grid with difference_finder returns whether there are legal moves, the number of white tiles, the number of black tiles, the move that was played in terms of (i and j), the differences(in terms of i) the differences(in terms of j) the new tiles they changes 

 
## Creating the start game menu (lines 1173-1606) 
•	Printer function updates the board, there are 3 frames: 1.) holds whose turn it is or whose won, 2.) shows the number of tiles for each side, 3.) shows the grid itself. Printer updates this with animations and allows the player to click to play their move.
•	At the end of the game the new_game screen appears which tells the player who won, with a score based on tiles and gives them the option to play the same game again or to go to the start screen to change the settings.
•	The start screen allows the player to decide on which AIs/the player plays purple and yellow and gives stats and descriptions, it also allows the player to select animation lengths, show all AIs with a button and create a pop-up with 3 slides that explains how to play the game.

### Functions:
•	printer(legal_moves_check, white_counter, black_counter, move_i, move_j, difference, difference_i, difference_j, animation_timer): there are 3 frames: 1.) holds whose turn it is or whose won (depending on legal_moves_check), 2.) shows the number of tiles for each side (using white_counter and black_counter), 3.) shows the grid itself and is updated with animations based on(difference, difference_i, difference_j, animation_timer, move_i and move_j) with the tile where the move is played briefly turning black with changed colours moving across a colour gradient. Printer updates this with animations and allows the player to click to play their move. If the AI is to play it allows them to with play_move(-2. -2, legal_moves_check, animation_timer), if it’s the player’s turn they play by clicking on a white tile on the board. It never returns
•	create_end_game_box(larger, smaller): at the end of the game create a box to display the score, there are buttons to run play_again_command, to run new_game_command or to minimise it with hide_command never returns
•	hide_command(main_frame) destroys the main frame (that shows the score) and creates a “show” button to bring it back never returns
•	show_command(self_frame, largest, smallest) re-creates the frame that shows the score with create_end_game_box and destroys itself never returns
•	play_again_command(master_frame): resets the grid to the start one with set_up_grid and difference_finder, destroys the frame and runs printer never returns
•	new_game_command() reset the grid and find legal moves with set_up_grid and legal_moves and create the start menu with create_game_frame_list and create_game_selection_screen never returns
•	start_game(frame_to_destroy): fetch the ai_strategies from results, decide how long animations will be, destroy the frame_to_destroy, create the grid. Runs play_move. It never returns
•	create_outside_frame_box(master_frame): create a box to hold the frames for the start-menu with the outside being frames with colour black to appear as an outline, set the size of rows and columns. It never returns.
•	create_title(master_frame): set the font and create the label for the title “Reversi!” to be placed in the title box it never returns
•	create_custom_drop_down_menu(master_frame, background_colour, strength_score, cunning_score, depth_score, random_score, description) I didn’t like the look of the standard drop-down menu so this replaces it. Inside of it two functions are defined:
•	Show_menu(event): this creates a menu that appears at the mouse when created filled with options, the options have the command select_option
•	Select_option(chosen_option): this updates the label with the chosen option and updates result as this is needed to pick the AI later, it then updates the stats
the drop-down menu creates a menu that allows the player to pick from a number of options, it then updates the rest of the information in the box and sets the result to be accessed later returns chosen¬_label and result
•	fill_chosen_player_box(master_frame, background_colour): this fills the box to show the chosen player for each side, it sets the column and row sizes, and uses new function place_stats(row, column, columnspan, font_size, text) which places a label with a specific font, font_size and background at a specific place in the grid taking up a certain number of columns and adds drop down menus with create_custom_drop_down_menu. Returns result
•	create_player_chooser_boxes(master_frame): creates the boxes that holds the choices of which AI/player plays each side, it uses a number of frames, the outside are black to create a border whilst the insides are to be filled later using fill_chosen_player_box returns the result_list so it can be used later with .get() to find out who is playing purple and yellow
•	create_animation_box(master_frame): create the box and the animation slider inside of it, return the animation_slider so its value can be accessed later with .get()
•	create_start_game_box(master_frame, master_master_frame) this box holds the start_game_button and runs start_game which destroys the master_master_frame never returns
•	show_all_ai_button(master_frame): create the button that when pressed adds all AIs to the options list so they can all be selected by running show_all_ai_command once pressed this disappears never returns
•	show_all_ai_command(self, master_frame): this command destroys itself and updates the global options never retuns
•	how_to_play_button(master_frame): this command creates a button with a specific font to be pressed which runs command how_to_play_command which has a number of frames explaining how to play the game never returns
•	close_how_to_play(master_frame): this command destroys the frame and sets the global how_to_play_flag to False so that a new how_to_play window can be created never returns
•	next_image(number): when ran this goes to the next number in the image list if there is one, and runs how_to_play_command with the new number and never returns
•	previous_image(number): when ran this goes to the previous  number in the image list if there is one, and runs how_to_play_command with the new number and never returns
•	how_to_play_command(current_image, internal): if the global how_to_play_flag is True and its not being run from an internal next_image or previous_image command it returns immediately otherwise it creates a new frame with 3 buttons to run close_how_to_play, previous_image and next_image and sets column and row sizes. Never returns
•	create_game_frame_list(): creates the global list of frames for the game that are used later. Never returns
•	create_game_selection_screen(): this creates the screen for the start of the game where all the settings can be chosen. It runs create_game_frame_list, create_outside_frame_box, create_title, create_player_chooser_boxes, create_animation_box, create_start_game_box, show_all_ai_button, how_to_play_button and never returns 

### Other:
•	The window for all tkinter stuff is created separately and is thus in global scope
•	Some global variables are set-up
•	Create_game_selection_screen() is ran
•	image paths for the how_to_play screen are created and saved
•	window_mainloop() is ran to start the programme



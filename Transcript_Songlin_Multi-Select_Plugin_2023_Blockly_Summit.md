Good afternoon and Good evening if you are joining this show virtually in Europe just like me! I'm Songlin Jiang, an Erasmus Mundus Master's Student currently studying at Aalto University in Finland and will continue my study at the Technical University of Denmark next semester. It’s a pity that I was not able to make my trip to the US and talk to you face to face because of the visa issue. Today, I will introduce you to the plugin I've worked on that allows Blockly to select, drag, and manipulate multiple blocks.

(Next slide)

By the end of this talk, we will have an overview of the multi-select plugin. I divide this talk into four parts. First, I will tell you some backgrounds of the plugin. Then, I will introduce you to the functionalities realized by this plugin. Later, I will tell you the structures of the plugin. Lastly, I will mention some issues that need further work for the multi-select plugin.

(Next slide)

Let's first talk about some backgrounds of this plugin. Why do we want multi-select in Blockly?

(Next slide)

First, it's the user's intuition. Everyone here must have experience with managing your files on the Operating System. You can click on the files while pressing the command or control key or drag to draw a rectangle to select multiple files. Then you can move them around, copying or deleting.

(Next slide)

It sounds a bit easy, but multi-selection can become a crazy-complex feature when considering the details. Nevertheless, people like this idea, and there was an issue requesting this feature as early as seven years ago.

(Next slide)

And it happens that MIT App Inventor also had such an idea for the project in Google Summer of Code twenty-twenty-two. So, then I went down into this rabbit hole and started developing this plugin.

(Next slide)

Now, let's see what this plugin can do.

(Next slide)

Hold the SHIFT key or click the multi-select icon, and we will enter the multi-select mode. We can select new blocks by clicking them. We can also deselect the block by clicking the already selected ones.

After we exit the multi-select mode by releasing the SHIFT key or clicking on the multi-select icon again, we can clear the selection by clicking on the workspace background or a random unselected block.

(Next slide)

When in multi-select mode, we can also drag to draw a rectangle to select blocks. We can select or deselect the blocks touched by the rectangle area.

In multi-select mode, workspace dragging and block dragging will all be disabled. You can only drag to draw a rectangle for selection.

(Next slide)

When we exit the multi-select mode, we can drag multiple blocks simultaneously. We also take good care of the connections in this case. For example, when some of the selected blocks are in one block stack, so some top blocks and some of their children blocks are in the selection simultaneously. The plugin only disconnects the selected topmost block in that stack with its parent block. Move along with all the children's blocks of that topmost block. When the drag is released, the blocks in the selection list will only try to connect to those blocks that are not in the selection list.

(Next slide)

We can delete multiple blocks using the keyboard, through the menu, or drag them to the trash can.

(Next slide)

When we edit the fields while selecting multiple blocks, we will automatically have the new value applied to all the blocks with the same type.

(Next slide)

We have adapted all the possible menu entries for block and workspace to multi-selection. We also add the new "select all blocks" workspace menu entry and the keyboard shortcut.

(Next slide)

We duplicate the selected topmost block in the block stack and all the children blocks of that topmost block. The selection will be changed to all newly created duplicated topmost blocks.

The actions to be applied in the menu are determined by the state of the block which the user right-clicks on, and the same state will be used for all the blocks no matter their state before. We append the estimated number of user-selected state-changing blocks to the end of the menu entry, and the number will only be shown when it is greater than 1.

(Next slide)

We have also integrated the cross-tab copy-and-paste multi-selected blocks feature into our plugin. We can copy, and cut the blocks in one tab, then paste them into another tab through workspace menus or keyboard shortcuts.

(Next slide)

The multi-select plugin can also integrate well with other Blockly plugins—for example, the edge scroll plugin.

(Next slide)

After learning so many features of the multi-select plugin, you must wonder how we implement this plugin into reality. So now I will tell you the general structure of this plugin to show how it works.

(Next slide)

We have three layers. The top layer is DragSelect, a third-party Javascript library that interacts directly with users, such as multi-select by clicking or drawing a rectangle. I submitted several Pull Requests to this library so that it can work for Blockly. DragSelect allows our plugin in layer 2 to know what blocks the user selects. The plugin will keep maintaining the multi-selection state for the Blockly core.

(Next slide)

Now look into layer 2 in detail. Our plugin acts like an adapter. It maintains its multi-selection set, which records currently selected blocks and ensures we always have one of our selected blocks as the selected one at the workspace in Blockly core. When users do some actions, the plugin also passes all the actions to the other blocks in our set beside the selected one at the workspace in Blockly core.

(Next slide)

Now that we have a basic idea of this plugin, let's check the to-do list for the plugin.

(Next slide)

There is one known issue with this plugin. Since DragSelect listens to the SVG path element to know whether a block is selected, the SVG path is a rectangle with some transparent parts forming a block. If you click on the transparent area within the SVG rectangle for irregularly shaped blocks, it will undesirably get selected. A proper fix is still under investigation, and maybe Blockly can implement some APIs that DragSelect can use to know where the block locates.

(Next slide)

Besides, there should be few issues, but more UI integration tests are still needed as you may never expect where the bugs would locate. Also, the plugin has some monkey patches code, so maybe related code in Blockly core can be refactored to allow more flexibility and to remove those monkey patches. In addition, there should be room for UI performance improvement in this plugin. Last but not least, although many other Blockly plugins can work well with the multi-select plugin, we will still need to check more to see if we need code modification to work with them.

(Next slide)

That's all for my talk. Thanks for listening. You can scan the QR code here to play with the live demo instance or access the source code repository. Any questions?

Area-Zone-Board
===============

We need to manage area to represent some spaces.

First idea provide from project : https://github.com/TOTOleHero/QBG---QuestBoardGame/wiki/Main-engine#roomconfiguration 

To simplify the management, we consider this area are rectangle with some squares tiles.

The first use is to represente dungeon floors like this : http://www.wqchronicles.com/boards/boards.htm

The first case is T floor with 4x6 size :

     - - - - - -
    |1|2|3|4|5|6|
     - - - - - -
    |2| | | | | |
     - - - - - -
    |3| | | | | |
     - - - - - -
    |4| | | | | |
     - - - - - -

Each tile square can be anything and we need to define what is the tile behaviour.

Floor are represented by Array of TileLine, containing Array of Tile, containing Array of TileFunction.

Example for T junction :

    definition : [
      [ [‘IO’], [‘N’], [‘N’],  [‘N’],  [‘N’], [‘IO’] ]
     ,[ [‘IO’], [‘N’], [‘N’],  [‘N’],  [‘N’], [‘IO’] ]
     ,[ [‘B’],  [‘B’], [‘N’],  [‘N’],  [‘B’], [‘B’]  ]
     ,[ [‘B’],  [‘B’], [‘IO’], [‘IO’], [‘B’], [‘B’]  ]
    ];

With :

    IO: InputOutputTile, to put a door
    N : NormalTile mode of Tile
    B : For BlankTile (in system None or null can be considered as B function)

If Tile must have more than on function you need to accumulate the TileFunction.

Exemple for stair floor InputOutputTile, the Tile must have two function [‘IO’,‘ST’] IO for InpuOutput and STair for step.

Tile definition
---------------

We need to define tile behaviour in file.


Exemple for chasm, we define chasm as CHM code


    code : CHM
    events :
    [
      {
          type : onWalkOn 
          actions : [
              {
                   subjectType : hero
                   test : @initiative < 1D6
                   targetElement : profil
                   elementType : wounds
                   result : -@wounds
              },
              {
                   subjectType : monster
                   test : true
                   targetElement : profil
                   elementType : wounds
                   result : -@wounds
              }

          ]
      },
      {
          type : onJumpAbove 
          actions : [
              {
                   targetType : hero
                   test : @initiative < 1D6
                   targetElement : event
                   elementType : null
                   result : onWalkOn
              }
          ]
      }

    ]

It is mandatory to define list of event, action, targetElement, ... can be managed be game.



# raylib-visual-studio-2022
How to make raylib static libraries for visual studio 2022 from raylib 4.0 source code.

1. Download the source code from https://github.com/raysan5/raylib/releases

2. At this time of writing, raylib v4.0.0 is available. You can download the v4.0 source code zip file directly from this link: https://github.com/raysan5/raylib/archive/refs/tags/4.0.0.zip

3. Extract the zip file, your extracted folder looks similar to this

![Extraction](/images/s01-extraction.png)

4. After that run the `CMake gui` and select the source and build folders. Then click on `Configure` to show Cmake options. Uncheck `BUILD_EXAMPLES` from the options. <br/>
<b>Note:</b> You need to install `Cmake` prior to this process.

![Cmake Gui](/images/s02-cmake.png)

5. And click on `Generate` button then choose `Visual Studio 17 2022` and `x64` as platform. And click on `Finish` to close the configuration window.

![Generate](/images/s03-config.png)

6. And then click on `Open Project` button to open `Visual studio` solution. 

7. From the opened solution, right click on `raylib` project and choose `Set as Startup Project` option. Next you can delete all Cmake generated projects if you want. You can even delete Cmake generated unwanted build configs like `MinSizeRel` etc. We only need `Debug` and `Release` configurations.

![Solution](/images/s04-solution.png)

8. Now right click on `raylib` project and choose `properties` set the `OutputDirectory` -> `$(SolutionDir)_Bin\$(Configuration)` for better visibility for output files. The for each platform (Debug and Release) make builds then the output folder looks like this. It contains `raylib.lin` files for each configuration.

![Solution](/images/s05-properties.png)

![Solution](/images/s06-build.png)

9. For better organization, go to project directory, create a new folder `Output` and copy `include` folder. And create another folder `lib` in that `Output` folder and copy newly generated `Debug` and `Release` folders. The folder structure looks like this. We are gonna use these files in our raylib application/game.

![Solution](/images/s07-output.png)

10. Now create a brand new `Empty C++ console` project. Create `main.cpp` file in that project to activate C++ options.

![Solution](/images/s08-empty.png)

12. Open the newly created project in explorer, copy and paste the Output directory contents into the solution root directory.

![Solution](/images/s09-copy.png)

11. Open project properties window and select `All Configurations` and set these options

    > `$(SolutionDir)include` under `C/C++ -> Additional Include Dirrectories`<br/>

    > `$(SolutionDir)lib\$(Configuration)` under `Linker/General -> Additonal Library Directories`<br/>

    > Add these under `Linker/Input/Additional Depencies`
        >> raylib.lib <br/>
        >> winmm.lib <br/>
        >> opengl32.lib <br/>
        >> kernel32.lib <br/>
        >> gdi32.lib <br/>

    > And `$(SolutionDir)_Bin\$(Configuration)` under General/Output Directory.




12. Then copy this code into main.cpp file and make both Debug and Release builds. You will get the builds under
`_Bin` folder in project directory.

```
#include "raylib.h"

//------------------------------------------------------------------------------------
// Program main entry point
//------------------------------------------------------------------------------------
int main(void)
{
    // Initialization
    //--------------------------------------------------------------------------------------
    const int screenWidth = 800;
    const int screenHeight = 600;

    InitWindow(screenWidth, screenHeight, "raylib - window tests");

    SetTargetFPS(60);               // Set our game to run at 60 frames-per-second
    //--------------------------------------------------------------------------------------

    // Main game loop
    while (!WindowShouldClose())    // Detect window close button or ESC key
    {
        // Update
        //----------------------------------------------------------------------------------
        // TODO: Update your variables here
        //----------------------------------------------------------------------------------

        // Draw
        //----------------------------------------------------------------------------------
        BeginDrawing();

        ClearBackground(BLACK);

        DrawText("Success!!", 190, 200, 20, WHITE);

        EndDrawing();
        //----------------------------------------------------------------------------------
    }

    // De-Initialization
    //--------------------------------------------------------------------------------------
    CloseWindow();        // Close window and OpenGL context
    //--------------------------------------------------------------------------------------

    return 0;
}
```



![Solution](/images/s10-final.png)


### **************** Happy Programming in C++ *******************


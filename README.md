# desktop-overlay
Some code which allows an openFrameworks window to be transparent, always render on top of everything else, and click-through. Only tested on Windows.

## main.cpp
First, open your `main.cpp` file.
## 1) Include the Desktop Window Manager API `dwmpapi.h` at the top of the file.
```c++
#include <dwmapi.h>
#pragma comment (lib, "dwmapi.lib")
```

## 2) Implement this function.
```c++
/// makes the window always on top (may want to find a cross-platform solution in the future)
void makeWindowOnTop(HWND& wd)
{
    // making window always stay on top
    SetWindowPos(wd, HWND_TOPMOST, 0, 0, 0, 0, SWP_NOMOVE | SWP_NOSIZE | SWP_NOACTIVATE);

    // making window transparent
    MARGINS margins;
    margins.cxLeftWidth = -1;
    SetWindowLong(wd, GWL_STYLE, WS_POPUP | WS_VISIBLE);
    DwmExtendFrameIntoClientArea(wd, &margins);

    // making window click-through
    SetWindowLongPtr(wd, GWL_EXSTYLE, WS_EX_LAYERED | WS_EX_TRANSPARENT);
}
```

## 3) Add these lines to `int main()`, between `ofSetupGL()` and `ofRunApp()`.
```c++
HWND wnd = WindowFromDC(wglGetCurrentDC());
makeWindowOnTop(wnd);
```

Altogether, a basic `main.cpp' should look something like this.
```c++
#include <dwmapi.h> // For transparent topmost window
#pragma comment (lib, "dwmapi.lib") //...

/// makes the window always on top (may want to find a cross-platform solution in the future)
void makeWindowOnTop(HWND& wd)
{
    // making window always stay on top
    SetWindowPos(wd, HWND_TOPMOST, 0, 0, 0, 0, SWP_NOMOVE | SWP_NOSIZE | SWP_NOACTIVATE);

    // making window transparent
    MARGINS margins;
    margins.cxLeftWidth = -1;
    SetWindowLong(wd, GWL_STYLE, WS_POPUP | WS_VISIBLE);
    DwmExtendFrameIntoClientArea(wd, &margins);

    // making window click-through
    SetWindowLongPtr(wd, GWL_EXSTYLE, WS_EX_LAYERED | WS_EX_TRANSPARENT);
}

int main()
{
  ofSetupOpenGL(1920,1080,OF_WINDOW);

  HWND wnd = WindowFromDC(wglGetCurrentDC());
  makeWindowOnTop(wnd);

  // this kicks off the running of my app
  ofRunApp(new ofApp());
}

```

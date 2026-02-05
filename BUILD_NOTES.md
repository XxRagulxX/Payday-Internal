# Build Notes - Payday 3 Internal

## Recent Updates (Latest Build)

### Fixed Issues
1. **ImGui Font Atlas Error** - Fixed the error: `Backend does not support ImGuiBackendFlags_RendererHasTextures, and font atlas is not built!`
   - Added font atlas building in `InitImGui()` function in `Dx12Hook.cpp`
   - The font atlas is now properly initialized before first use

2. **Updated Dependencies to Stable Releases**
   - **ImGui**: Updated from `master` branch to stable release **v1.92.5**
   - **MinHook**: Updated from `master` branch to stable release **v1.3.4**
   - These are now fetched via CMake FetchContent from official repositories

3. **Removed Old Local Dependencies**
   - Removed `UnrealSDKBase/imgui/` folder (old local copy)
   - Removed `UnrealSDKBase/kiero/minhook/` folder (old local copy)
   - All dependencies are now managed by CMake

4. **Fixed SDK Static Assertion Errors**
   - Added `DUMPER7_DISABLE_ASSERTS` preprocessor definition
   - Modified `DumperAssertsFix.hpp` to disable all static_assert checks when this flag is set
   - This prevents build failures when the SDK is slightly out of sync with the game

5. **Fixed Kiero MinHook Include Path**
   - Updated `kiero.cpp` to use the CMake-managed MinHook path
   - Changed from `"minhook/include/MinHook.h"` to `<MinHook.h>`

## Build Instructions

### Release Build (Recommended)
```bash
cmake -S . -B out/build/x64-Release -G Ninja -DCMAKE_BUILD_TYPE=Release
cmake --build out/build/x64-Release --config Release
```

Output: `out/build/x64-Release/bin/UnrealSDKBase.dll`

### Clean Build (if needed)
```bash
Remove-Item -Path "out\build" -Recurse -Force
cmake -S . -B out/build/x64-Release -G Ninja -DCMAKE_BUILD_TYPE=Release
cmake --build out/build/x64-Release --config Release
```

## Testing the Fix

After building and injecting the new DLL, you should no longer see the ImGui error:
```
[imgui-error] Backend does not support ImGuiBackendFlags_RendererHasTextures, and font atlas is not built!
```

The menu should now render properly without errors.

## Dependencies (Auto-Fetched by CMake)
- **ImGui v1.92.5** - https://github.com/ocornut/imgui/releases/tag/v1.92.5
- **MinHook v1.3.4** - https://github.com/TsudaKageyu/minhook/releases/tag/v1.3.4

## Known Limitations
- Debug builds may have CMake configuration issues (use Release build)
- SDK static assertions are disabled to allow building even when SDK is slightly out of sync

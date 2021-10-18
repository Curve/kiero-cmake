
<h1 align="center"> kiero-cmake </h1>
<p align="center">
A CMake compatible version of <a href="https://github.com/Rebzzel/kiero">kiero</a>, no manual header manipulation required!
</p>


---

## ⚙️ Configuration
### Backend-Type
```cmake
set(kiero_backend "<backend-type>")
```
where <kbd>backend-type</kbd> is one of `D3D9`,`D3D10`,`D3D11`,`D3D12`,`OpenGL`,`Vulkan`.
### MinHook
```cmake
set(kiero_minhook ON)
```
> If this option is set to true `find_package(minhook)` is ran, if minhook is then found it will be linked, otherwise it is up to the user to link minhook.

## 📎 Installation
- FetchContent
    ```cmake
    include(FetchContent)
    FetchContent_Declare(kiero GIT_REPOSITORY "https://github.com/Curve/kiero")

    FetchContent_MakeAvailable(kiero)
    target_link_libraries(<YourLibrary> kiero)
    ```
- Git Submodule
    ```bash
    git submodule add "https://github.com/Curve/kiero"
    ```
    ```cmake
    # Somewhere in your CMakeLists.txt
    add_subdirectory("<path_to_kiero>")
    target_link_libraries(<YourLibrary> kiero)
    ```

## 📓 Original Readme
The original project and readme can be found [here](https://github.com/Rebzzel/kiero#readme).
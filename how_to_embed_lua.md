# How to embed Lua

This will show how to embed Lua into a C/C++ project.

## Prerequisites

- Lua
    - lua.h
    - lualib.h
    - lauxlib.h
- C/C++

## lua_State

Firstly, we need to create a lua_State struct. For that we have the `luaL_newstate` function.
Just simply call the function:
```c
lua_State *L = luaL_newstate();
```

Calling the state `L` is a convention you may follow.

## Execute a file

For executing a file we have the `luaL_dofile` function.
It takes the state and a file path to a Lua file and returns if it successfully executed the file or not:
```c
int res = luaL_dofile(L, "main.lua");
if (res != LUA_OK) {
    const char* msg = lua_tostring(L, -1);
    fprintf(stderr, "Lua Error: %s\n", msg);
}
```

## Getting values from Lua

There are many things we can get from Lua. Here are some important functions:
- `lua_getglobal`, to load a global variable on the top of the stack
- `lua_getfield`, to load a field of a table on the top of the stack
- `lua_toboolean`
- `lua_tocfunction`
- `lua_touserdata`
- `lua_tothread`
- `lua_topointer`
- `lua_tonumber`
- `lua_tointeger`
- `lua_tostring`
- `lua_tounsigned`

It is important to verify that the type of the value is correct before using it. You can do that with these functions:
- `lua_isnumber`
- `lua_isstring`
- `lua_iscfunction`
- `lua_isinteger`
- `lua_isuserdata`
- `lua_isfunction`
- `lua_istable`
- `lua_islightuserdata`
- `lua_isnil`
- `lua_isboolean`
- `lua_isthread`
- `lua_isnone`
- `lua_isnoneornil`

## Functions

Registering a C function:
```c
lua_register(L, "FunctionNameToUseInLua", function_implemented_in_c);
```

Get a function from Lua and call it:
```c
lua_getglobal(L, "FunctionName");
if (lua_isfunction(L, -1)) { // verify, that is is indeed a function
    // push arguments
    lua_pushnumber(L, 1);

    // call the function
    int res = lua_pcall(L, 1, 0, 0); // lua_pcall(state, amount of arguments, amount of results, error handler)
    if (res != LUA_OK) {
        const char* msg = lua_tostring(L, -1);
        fprintf(stderr, "Lua Error: %s\n", msg);
    }
}
```

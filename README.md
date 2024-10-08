<h1 align="center"><img src="misc/logo.png" width="600" height="126" alt="LuaPreprocess" title="LuaPreprocess"></h1>

**LuaPreprocess** - a small and straightforward Lua preprocessor featuring a simple syntax.
Write embedded metaprograms to generate code using normal Lua inside your Lua files.

LuaPreprocess is written in pure Lua.
[The library](preprocess.lua) is a single file with no external dependencies.
[MIT license](LICENSE.txt).
A separate [command line program](preprocess-cl.lua) is available too.

- [Example Program](#example-program)
- [Usage](#usage)
	- [Library](#preprocess-files-using-the-library)
	- [Command Line](#preprocess-files-from-the-command-line)
- [Documentation](docs/)
- [Help](#help)



## Example Program

The exclamation mark (`!`) is used to indicate what code is part of the metaprogram.
([See screenshot of processing steps with highlighting](https://github.com/GamesRightMeow/LuaPreprocess/blob/master/misc/processingSteps.png))

```lua
-- Normal Lua.
local n = 0
doSomething()

-- Preprocessor lines.
!local IS_DEVELOPER = true
initGame()
!if IS_DEVELOPER then
	enableCheats()
!else
	enableTelemetry()
!end

function newArrayOfThreeBits()
	return {
		!for i = 1, 3 do
			0,
		!end
	}
end

-- Extended preprocessor line. (Lines are consumed until brackets
-- are balanced when the end of the line is reached.)
!defineClass{
	name  = "Entity",
	props = {x=0, y=0},
}

-- Preprocessor block.
!(
local hashLib = require("md5")
function getHash()
	return hashLib.calculate("Hello, world!")
end
)

-- Preprocessor inline block. (Expression that returns a value.)
local text = !("Precalculated hash: "..getHash())

-- Preprocessor inline block variant. (Expression that returns a Lua code string.)
!!("myRandomGlobal"..math.random(9)) = "foo"
```

#### Output

```lua
-- Normal Lua.
local n = 0
doSomething()

-- Preprocessor lines.
initGame()
enableCheats()

function newArrayOfThreeBits()
	return {
		0,
		0,
		0,
	}
end

-- Extended preprocessor line. (Lines are consumed until brackets
-- are balanced when the end of the line is reached.)
function newEntity() return {__name="Entity",x=0,y=0} end

-- Preprocessor block.

-- Preprocessor inline block. (Expression that returns a value.)
local text = "Precalculated hash: 6CD3556DEB0DA54BCA060B4C39479839"

-- Preprocessor inline block variant. (Expression that returns a Lua code string.)
myRandomGlobal4 = "foo"
```

See the [examples folder](examples) for more.
(See also [an example for the LÖVE framework](https://gist.github.com/ReFreezed/be97dce6b67496b0f0c5275bf2a96d51).)



## Usage

First you need [Lua](https://www.lua.org/) installed on your system. (Binaries can be
downloaded from [LuaBinaries via SourceForge](https://sourceforge.net/projects/luabinaries/files/5.1.5/Tools%20Executables/)
if you don't want to, or can't, compile Lua from source. For Windows I can recommend installing
[LuaForWindows](https://github.com/rjpcomputing/luaforwindows) which is a "batteries included" Lua package.)


### Preprocess Files Using the Library

```lua
local pp = require("preprocess")

local info, err = pp.processFile{
	pathIn   = "src/app.lua2p",     -- This is the file we want to process.
	pathOut  = "output/app.lua",    -- The output path.
	pathMeta = "temp/app.meta.lua", -- Temporary output file for the metaprogram.
}

if not info then
	error(err)
end

print("Lines of code processed: "..info.lineCount)
```

See the [docs](docs/)
or the top of [preprocess.lua](preprocess.lua) for documentation.


### Preprocess Files from the Command Line

#### Windows

```batch
Preprocess.cmd [options] filepath1 [filepath2 ...]
OR
Preprocess.cmd --outputpaths [options] inputpath1 outputpath1 [inputpath2 outputpath2 ...]
```

#### Any System

```batch
lua preprocess-cl.lua [options] filepath1 [filepath2 ...]
OR
lua preprocess-cl.lua --outputpaths [options] inputpath1 outputpath1 [inputpath2 outputpath2 ...]
```

If a filepath is, for example, `C:/MyApp/app.lua2p` then LuaPreprocess will write the processed file to `C:/MyApp/app.lua`.

See the [docs](docs/),
or the top of [preprocess-cl.lua](preprocess-cl.lua)
and [preprocess.lua](preprocess.lua), for the options and more documentation.



## Documentation

- [Docs](docs/)
- Library: See the top of [preprocess.lua](preprocess.lua)
- Command line: See the top of [preprocess-cl.lua](preprocess-cl.lua)



## Help

Got a question?
If the [documentation](docs/) doesn't have the answer,
look if someone has asked the question in the [issue tracker](https://github.com/GamesRightMeow/LuaPreprocess/issues),
or [create a new issue](https://github.com/GamesRightMeow/LuaPreprocess/issues/new).



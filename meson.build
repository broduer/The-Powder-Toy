project(
	'the-powder-toy',
	['c', 'cpp'],
	version: '94.1.343' # must be of the form 'major.minor.build.whatever'
)

cpp_compiler = meson.get_compiler('cpp')

common_version = meson.project_version().split('.')
project_cpp_arg = [
	'-D_64BIT',
	'-DLIN',
	'-DSAVE_VERSION=' + common_version[0],
	'-DMINOR_VERSION=' + common_version[1],
	'-DBUILD_NUM=' + common_version[2],
	'-DDEBUG',
	'-DX86',
	'-DX86_SSE',
	'-DX86_SSE2',
	'-DX86_SSE3',
	'-DSDL_INC',
#	'-DOGLI',
#	'-DOGLR',
#	'-DPIX32OGL',
#	'-DSNAPSHOT',
#	'-DBETA',
#	'-DNO_INSTALL_CHECK',
#	'-DNOHTTP',
	'-DSNAPSHOT_ID=0',
	'-DMOD_ID=0',
	'-msse',
	'-msse2',
	'-msse3',
	'-march=native',
]

project_inc = include_directories([
	'src',
	'data',
	'generated',
])
project_link_arg = [
	'-m64',
#	'-m32',
]
project_dep = [
	dependency('threads')
]

simulation_elem_src = []
foreach elem : [
   '116',   '146',  'ACEL',  'ACID',  'AMTR',  'ANAR',  'ARAY',  'BANG',
  'BCLN',  'BCOL',  'BGLA',  'BHOL',  'BIZR', 'BIZRG', 'BIZRS',  'BMTL',
  'BOMB',  'BOYL',  'BRAY',  'BRCK',  'BREC',  'BRMT',  'BTRY',  'BVBR',
    'C5',  'CAUS',  'CBNW',  'CFLM',  'CLNE',  'CLST',  'CNCT',   'CO2',
  'COAL',  'CONV',  'CRAY',  'CRMC',  'DCEL',  'DESL',  'DEST',  'DEUT',
  'DLAY',   'DMG',  'DMND',  'DRAY',  'DRIC',  'DSTW',  'DTEC',  'DUST',
  'DYST',  'ELEC',  'EMBR',   'EMP',  'ETRD',  'EXOT',  'FIGH',  'FILT',
  'FIRE',  'FIRW',   'FOG',  'FRAY',  'FRME',  'FRZW',  'FRZZ',  'FSEP',
  'FUSE',  'FWRK',   'GAS',  'GBMB',   'GEL',  'GLAS',  'GLOW',  'GOLD',
   'GOO',  'GPMP',  'GRAV',  'GRVT',  'GUNP',    'H2',  'HEAC',  'HSWC',
  'ICEI',  'IGNT',  'INSL',  'INST', 'INVIS',  'INWR',  'IRON',  'ISOZ',
  'ISZS',  'LAVA',  'LCRY',  'LDTC',  'LIFE',  'LIGH',  'LNTG',   'LO2',
  'LOLZ',  'LOVE',  'LRBD',  'LSNS',  'MERC',  'METL',  'MORT',  'MWAX',
  'NBHL',  'NBLE',  'NEUT',  'NICE',  'NITR',  'NONE',  'NSCN',  'NTCT',
  'NWHL',    'O2',   'OIL',  'PBCN',  'PCLN',  'PHOT',  'PIPE',  'PLEX',
  'PLNT',  'PLSM',  'PLUT',  'POLO',  'PPIP',  'PQRT',  'PROT',  'PRTI',
  'PRTO',  'PSCN',  'PSNS',  'PSTE',  'PSTN',  'PSTS',  'PTCT',  'PUMP',
  'PVOD',  'QRTZ',  'RBDM',  'RFGL',  'RFRG',  'RIME',  'RPEL',  'SALT',
  'SAND',  'SAWD', 'SHLD1', 'SHLD2', 'SHLD3', 'SHLD4',  'SING',  'SLTW',
  'SMKE',  'SNOW',  'SOAP', 'SPAWN','SPAWN2',  'SPNG',  'SPRK',  'STKM',
 'STKM2',  'STNE',  'STOR',  'SWCH',  'TESC',  'THDR',  'THRM',  'TRON',
  'TSNS',  'TTAN',  'TUNG',  'URAN',  'VIBR',  'VINE',  'VIRS',  'VOID',
  'VRSG',  'VRSS',  'WARP',  'WATR',   'WAX',  'WHOL',  'WIFI',  'WIRE',
  'WOOD',  'WTRV',  'YEST'
]
	simulation_elem_src += 'src/simulation/elements/' + elem + '.cpp'
endforeach

simulation_tool_src = [
	'src/simulation/simtools/Cool.cpp',
	'src/simulation/simtools/Cyclone.cpp',
	'src/simulation/simtools/NGrv.cpp',
	'src/simulation/simtools/AirTool.cpp',
	'src/simulation/simtools/Mix.cpp',
	'src/simulation/simtools/Vac.cpp',
	'src/simulation/simtools/PGrv.cpp',
	'src/simulation/simtools/Heat.cpp',
	'src/simulation/simtools/SimTool.cpp',
]

simulation_cpp_arg = [
	'-DGRAVFFT',
]
simulation_lib = static_library(
	'simulation',
	sources: [
		'src/simulation/Air.cpp',
		'src/simulation/Element.cpp',
		'src/simulation/Gravity.cpp',
		'src/simulation/Particle.cpp',
		'src/simulation/SaveRenderer.cpp',
		'src/simulation/Sign.cpp',
		'src/simulation/SimulationData.cpp',
		simulation_elem_src,
		simulation_tool_src,
	],
	dependencies: [
		project_dep,
		dependency('fftw3f'),
	],
	include_directories: project_inc,
	cpp_args: [
		project_cpp_arg,
		simulation_cpp_arg,
	],
	link_args: project_link_arg,
)
simulation_dep = declare_dependency(link_with: simulation_lib)

client_src = [
	'src/client/MD5.cpp',
	'src/client/SaveFile.cpp',
	'src/client/SaveInfo.cpp',
	'src/client/ThumbnailRendererTask.cpp',
]

http_src = [
	'src/client/http/APIRequest.cpp',
	'src/client/http/AvatarRequest.cpp',
	'src/client/http/GetUserInfoRequest.cpp',
	'src/client/http/ImageRequest.cpp',
	'src/client/http/Request.cpp',
	'src/client/http/RequestManager.cpp',
	'src/client/http/SaveUserInfoRequest.cpp',
	'src/client/http/ThumbnailRequest.cpp',
]

graphics_lib = static_library(
	'graphics',
	sources: [
		'src/graphics/Graphics.cpp',
		'src/graphics/OpenGLGraphics.cpp',
		'src/graphics/RasterGraphics.cpp',
		'src/resampler/resampler.cpp',
	],
	dependencies: project_dep,
	include_directories: project_inc,
	cpp_args: project_cpp_arg,
	link_args: project_link_arg,
)
graphics_dep = declare_dependency(link_with: graphics_lib)

common_lib = static_library(
	'common',
	sources: [
		'data/font.cpp',
		'data/hmap.cpp',
		'data/icon.cpp',
		'data/images.cpp',
		'src/bson/BSON.cpp',
		'src/json/jsoncpp.cpp',
		'src/common/String.cpp',
		'src/common/tpt-rand.cpp',
		'src/Format.cpp',
		'src/Misc.cpp',
		'src/Platform.cpp',
		'src/Probability.cpp',
	],
	dependencies: project_dep,
	include_directories: project_inc,
	cpp_args: project_cpp_arg,
	link_args: project_link_arg,
)
common_dep = declare_dependency(link_with: common_lib)

luaconsole_src = [
	'src/lua/CommandInterface.cpp',
	'src/lua/LegacyLuaAPI.cpp',
	'src/lua/LuaBit.cpp',
	'src/lua/LuaButton.cpp',
	'src/lua/LuaCheckbox.cpp',
	'src/lua/LuaCompat.c',
	'src/lua/LuaComponent.cpp',
	'src/lua/LuaEvents.cpp',
	'src/lua/LuaLabel.cpp',
	'src/lua/LuaProgressBar.cpp',
	'src/lua/LuaScriptInterface.cpp',
	'src/lua/luascripts/eventcompat.lua.cpp',
	'src/lua/LuaSlider.cpp',
	'src/lua/LuaSmartRef.cpp',
	'src/lua/LuaTextbox.cpp',
	'src/lua/LuaWindow.cpp',
	'src/lua/socket/auxiliar.c',
	'src/lua/socket/buffer.c',
	'src/lua/socket/except.c',
	'src/lua/socket/inet.c',
	'src/lua/socket/io.c',
	'src/lua/socket/luasocket.c',
	'src/lua/socket/options.c',
	'src/lua/socket/select.c',
	'src/lua/socket/socket.lua.cpp',
	'src/lua/socket/tcp.c',
	'src/lua/socket/timeout.c',
	'src/lua/socket/udp.c',
	'src/lua/socket/unix.c',
	'src/lua/socket/usocket.c',
	'src/lua/socket/wsocket.c',
	'src/lua/TPTScriptInterface.cpp',
	'src/lua/TPTSTypes.cpp',
]

interface_lib = static_library(
	'interface',
	sources: [
		'src/gui/interface/Appearance.cpp',
		'src/gui/interface/Button.cpp',
		'src/gui/interface/Checkbox.cpp',
		'src/gui/interface/Component.cpp',
		'src/gui/interface/ContextMenu.cpp',
		'src/gui/interface/DropDown.cpp',
		'src/gui/interface/Engine.cpp',
		'src/gui/interface/Label.cpp',
		'src/gui/interface/Panel.cpp',
		'src/gui/interface/ProgressBar.cpp',
		'src/gui/interface/ScrollPanel.cpp',
		'src/gui/interface/Slider.cpp',
		'src/gui/interface/Spinner.cpp',
		'src/gui/interface/Textbox.cpp',
		'src/gui/interface/TextWrapper.cpp',
		'src/gui/interface/Window.cpp',
		'src/gui/Style.cpp',
		'src/gui/dialogues/ConfirmPrompt.cpp',
	],
	dependencies: [
		project_dep,
		dependency('sdl2'),
		dependency('GL'),
		dependency('x11'),
	],
	include_directories: project_inc,
	cpp_args: project_cpp_arg,
	link_args: project_link_arg,
)
interface_dep = declare_dependency(link_with: interface_lib)

executable(
	'powder',
	sources: [
		'src/debug/DebugLines.cpp',
		'src/debug/DebugParts.cpp',
		'src/debug/ElementPopulation.cpp',
		'src/debug/ParticleDebug.cpp',
		'src/gui/colourpicker/ColourPickerActivity.cpp',
		'src/gui/console/ConsoleController.cpp',
		'src/gui/console/ConsoleModel.cpp',
		'src/gui/console/ConsoleView.cpp',
		'src/gui/dialogues/ErrorMessage.cpp',
		'src/gui/dialogues/InformationMessage.cpp',
		'src/gui/dialogues/SaveIDMessage.cpp',
		'src/gui/dialogues/TextPrompt.cpp',
		'src/gui/elementsearch/ElementSearchActivity.cpp',
		'src/gui/filebrowser/FileBrowserActivity.cpp',
		'src/gui/game/BitmapBrush.cpp',
		'src/gui/game/Brush.cpp',
		'src/gui/game/DecorationTool.cpp',
		'src/gui/game/Favorite.cpp',
		'src/gui/game/GameController.cpp',
		'src/gui/game/GameModel.cpp',
		'src/gui/game/GameView.cpp',
		'src/gui/game/Menu.cpp',
		'src/gui/game/PropertyTool.cpp',
		'src/gui/game/QuickOptions.cpp',
		'src/gui/game/SampleTool.cpp',
		'src/gui/game/SignTool.cpp',
		'src/gui/game/ToolButton.cpp',
		'src/gui/game/Tool.cpp',
		'src/gui/interface/AvatarButton.cpp',
		'src/gui/interface/CopyTextButton.cpp',
		'src/gui/interface/RichLabel.cpp',
		'src/gui/interface/SaveButton.cpp',
		'src/gui/localbrowser/LocalBrowserController.cpp',
		'src/gui/localbrowser/LocalBrowserModel.cpp',
		'src/gui/localbrowser/LocalBrowserView.cpp',
		'src/gui/login/LoginController.cpp',
		'src/gui/login/LoginModel.cpp',
		'src/gui/login/LoginView.cpp',
		'src/gui/options/OptionsController.cpp',
		'src/gui/options/OptionsModel.cpp',
		'src/gui/options/OptionsView.cpp',
		'src/gui/preview/PreviewController.cpp',
		'src/gui/preview/PreviewModel.cpp',
		'src/gui/preview/PreviewView.cpp',
		'src/gui/profile/ProfileActivity.cpp',
		'src/gui/render/RenderController.cpp',
		'src/gui/render/RenderModel.cpp',
		'src/gui/render/RenderView.cpp',
		'src/gui/save/LocalSaveActivity.cpp',
		'src/gui/save/ServerSaveActivity.cpp',
		'src/gui/search/SearchController.cpp',
		'src/gui/search/SearchModel.cpp',
		'src/gui/search/SearchView.cpp',
		'src/gui/tags/TagsController.cpp',
		'src/gui/tags/TagsModel.cpp',
		'src/gui/tags/TagsView.cpp',
		'src/gui/update/UpdateActivity.cpp',
		'src/powdertoyjava/OpenGLCanvasWin32.cpp',
		'src/powdertoyjava/PowderToyJava.cpp',
		'src/SDLCompat.cpp',
		'src/tasks/AbandonableTask.cpp',
		'src/tasks/Task.cpp',
		'src/tasks/TaskWindow.cpp',
		'src/Update.cpp',
		client_src,
		http_src,
		luaconsole_src,
		'src/graphics/Renderer.cpp', # see [note 1]
		'src/simulation/Simulation.cpp', # see [note 1]
		'src/client/Client.cpp', # see [note 1]
		'src/client/GameSave.cpp', # see [note 1]
		'src/PowderToySDL.cpp', # see [note 1]
		'generated/ElementClasses.cpp',
		'generated/ToolClasses.cpp',
	],
	dependencies: [
		project_dep,
		dependency('luajit'),
		dependency('zlib'),
		dependency('libcurl'),
		cpp_compiler.find_library('m'),
		cpp_compiler.find_library('dl'),
		cpp_compiler.find_library('bz2'),
		cpp_compiler.find_library('rt'),
		graphics_dep,
		common_dep,
		simulation_dep,
		interface_dep,
	],
	include_directories: project_inc,
	cpp_args: [
		project_cpp_arg,
		simulation_cpp_arg,
		'-DLUAJIT',
		'-DLUA_R_INCL',
		'-DLUACONSOLE',
#		'-DLUA_COMPAT_ALL',
	],
	link_args: project_link_arg,
)

executable(
	'render',
	sources: [
		'src/graphics/Renderer.cpp', # see [note 1]
		'src/simulation/Simulation.cpp', # see [note 1]
		'src/client/GameSave.cpp', # see [note 1]
		'src/PowderToyRenderer.cpp', # see [note 1]
		'generated/ElementClasses.cpp',
		'generated/ToolClasses.cpp',
	],
	dependencies: [
		project_dep,
		dependency('zlib'),
		cpp_compiler.find_library('bz2'),
		graphics_dep,
		common_dep,
		simulation_dep,
	],
	include_directories: project_inc,
	cpp_args: [
		project_cpp_arg,
		simulation_cpp_arg,
		'-DRENDERER'
	],
	link_args: project_link_arg,
)

executable(
	'font',
	sources: [
		'src/graphics/Renderer.cpp', # see [note 1]
		'src/gui/font/FontEditor.cpp', # see [note 1]
		'src/PowderToyFontEditor.cpp', # see [note 1]
	],
	dependencies: [
		project_dep,
		dependency('zlib'),
		cpp_compiler.find_library('bz2'),
		graphics_dep,
		common_dep,
		interface_dep,
	],
	include_directories: project_inc,
	cpp_args: [
		project_cpp_arg,
		'-DFONTEDITOR',
		'-DNOHTTP',
	],
	link_args: project_link_arg,
)

# [note 1]: these source files depend on the RENDERER and FONTEDITOR macros,
#           i.e. they behave differently based on which target is using them,
#           so they can't be part of the common static libraries

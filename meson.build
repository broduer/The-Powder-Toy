project('the-powder-toy', ['c', 'cpp'])

cpp_compiler = meson.get_compiler('cpp')

uopt_static = get_option('static')
uopt_platform = get_option('system')
if uopt_platform == 'auto'
	if host_machine.system() in ['linux', 'freebsd']
		uopt_platform = 'linux'
	elif host_machine.system() in ['windows']
		uopt_platform = 'windows'
	elif host_machine.system() in ['darwin']
		uopt_platform = 'mac'
	else
		error('unsupported platform')
	endif
endif
copt_x86 = host_machine.cpu_family() in ['x86_64', 'x86']
copt_64bit = host_machine.cpu_family() in ['x86_64', 'aarch64']
uopt_native = get_option('native')
uopt_x86_sse = get_option('x86_sse')
if uopt_x86_sse == 'auto'
	uopt_x86_sse_level = 20
	# idk lol
	#if copt_64bit
	#	uopt_x86_sse_level = 20
	#else
	#	uopt_x86_sse_level = 0
	#endif
elif uopt_x86_sse == 'sse3'
	uopt_x86_sse_level = 30
elif uopt_x86_sse == 'sse2'
	uopt_x86_sse_level = 20
elif uopt_x86_sse == 'sse'
	uopt_x86_sse_level = 10
elif uopt_x86_sse == 'none'
	uopt_x86_sse_level = 0
endif
if not copt_x86 or uopt_native
	uopt_x86_sse_level = 0
endif
uopt_lua = get_option('lua')
if uopt_lua == 'luajit'
	lua_opt_dep = [ dependency('luajit', static: uopt_static) ]
elif uopt_lua == 'lua5.2'
	lua_opt_dep = [ dependency('lua5.2', static: uopt_static) ]
elif uopt_lua == 'lua5.1'
	lua_opt_dep = [ dependency('lua5.1', static: uopt_static) ]
else
	lua_opt_dep = []
endif
uopt_http = get_option('http')
if uopt_http
	curl_opt_dep = [ dependency('libcurl', static: uopt_static) ]
else
	curl_opt_dep = []
endif
uopt_fftw = get_option('gravfft')
if uopt_fftw
	fftw_opt_dep = [ dependency('fftw3f', static: uopt_static) ]
else
	fftw_opt_dep = []
endif

conf_data = configuration_data()
conf_data.set('NOHTTP', not uopt_http)
conf_data.set('OGLI', false)
conf_data.set('OGLR', false)
conf_data.set('PIX32OGL', false)
conf_data.set('BETA', get_option('beta'))
conf_data.set('NO_INSTALL_CHECK', not get_option('install_check'))
conf_data.set('GRAVFFT', uopt_fftw)
conf_data.set('LUACONSOLE', uopt_lua != 'none')
conf_data.set('LIN', uopt_platform == 'linux')
conf_data.set('WIN', uopt_platform == 'windows')
conf_data.set('X86', copt_x86)
conf_data.set('X86_SSE3', uopt_x86_sse_level >= 30)
conf_data.set('X86_SSE2', uopt_x86_sse_level >= 20)
conf_data.set('X86_SSE', uopt_x86_sse_level >= 10)
conf_data.set('NATIVE', uopt_native)
conf_data.set('_64BIT', copt_64bit)
conf_data.set('SAVE_VERSION', get_option('version_major'))
conf_data.set('MINOR_VERSION', get_option('version_minor'))
conf_data.set('BUILD_NUM', get_option('version_build'))
conf_data.set('MOD_ID', get_option('mod_id'))
conf_data.set('DEBUG', get_option('debug'))
conf_data.set('SNAPSHOT', get_option('snapshot'))
conf_data.set('SNAPSHOT_ID', get_option('snapshot_id'))
conf_data.set('FUTURE_SAVE_VERSION', get_option('future_major'))
conf_data.set('FUTURE_MINOR_VERSION', get_option('future_minor'))
config_inc = configure_file(
	input: 'src/Config.h.in',
	output: 'Config.h',
	configuration: conf_data
)

project_cpp_arg = []
if uopt_x86_sse_level >= 30
	project_cpp_arg += '-msse3'
endif
if uopt_x86_sse_level >= 20
	project_cpp_arg += '-msse2'
endif
if uopt_x86_sse_level >= 10
	project_cpp_arg += '-msse'
endif
if uopt_native
	project_cpp_arg += '-march=native'
endif
project_inc = include_directories([
	'src',
	'data',
	'generated',
])
project_dep = [
	dependency('threads', static: uopt_static),
	dependency('zlib', static: uopt_static),
	lua_opt_dep,
	curl_opt_dep,
	cpp_compiler.find_library('bz2', static: uopt_static),
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
		fftw_opt_dep,
	],
	include_directories: project_inc,
	cpp_args: project_cpp_arg,
)
simulation_dep = declare_dependency(link_with: simulation_lib)

client_src = [
	'src/client/http/APIRequest.cpp',
	'src/client/http/AvatarRequest.cpp',
	'src/client/http/GetUserInfoRequest.cpp',
	'src/client/http/ImageRequest.cpp',
	'src/client/http/Request.cpp',
	'src/client/http/RequestManager.cpp',
	'src/client/http/SaveUserInfoRequest.cpp',
	'src/client/http/ThumbnailRequest.cpp',
	'src/client/MD5.cpp',
	'src/client/SaveFile.cpp',
	'src/client/SaveInfo.cpp',
	'src/client/ThumbnailRendererTask.cpp',
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
		dependency('sdl2', static: uopt_static),
	],
	include_directories: project_inc,
	cpp_args: project_cpp_arg,
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
#		cpp_compiler.find_library('m'),
#		cpp_compiler.find_library('dl'),
#		cpp_compiler.find_library('rt'),
#		dependency('x11'),
#		dependency('GL'),
		project_dep,
		graphics_dep,
		common_dep,
		simulation_dep,
		interface_dep,
	],
	include_directories: project_inc,
	cpp_args: project_cpp_arg,
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
		graphics_dep,
		common_dep,
		simulation_dep,
	],
	include_directories: project_inc,
	cpp_args: [
		project_cpp_arg,
		'-DRENDERER'
	],
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
		graphics_dep,
		common_dep,
		interface_dep,
	],
	include_directories: project_inc,
	cpp_args: [
		project_cpp_arg,
		'-DFONTEDITOR',
	],
)

# [note 1]: these source files depend on the RENDERER and FONTEDITOR macros,
#           i.e. they behave differently based on which target is using them,
#           so they can't be part of the common static libraries


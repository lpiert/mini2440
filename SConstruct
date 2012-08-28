import os
import sys
import rtconfig

if os.getenv('RTT_ROOT'):
    RTT_ROOT = os.getenv('RTT_ROOT')
else:
    RTT_ROOT = os.path.normpath(os.getcwd() + '/../..')

sys.path = sys.path + [os.path.join(RTT_ROOT, 'tools')]
from building import *

TARGET = 'rtthread-mini2440.' + rtconfig.TARGET_EXT

env = Environment(tools = ['mingw'],
	AS = rtconfig.AS, ASFLAGS = rtconfig.AFLAGS,
	CC = rtconfig.CC, CCFLAGS = rtconfig.CFLAGS,
	CXX = rtconfig.CXX,
	AR = rtconfig.AR, ARFLAGS = '-rc',
	LINK = rtconfig.LINK, LINKFLAGS = rtconfig.LFLAGS)
env.PrependENVPath('PATH', rtconfig.EXEC_PATH)

Export('RTT_ROOT')
Export('rtconfig')

# prepare building environment
RTT_RTGUI = os.getenv('RTT_RTGUI')
if RTT_RTGUI:
	# if GUI dir is set to other place, don't use the one in RTT_ROOT
    objs = PrepareBuilding(env, RTT_ROOT, has_libcpu=False, remove_components=['rtgui'])
    objs += SConscript(os.path.join(RTT_RTGUI, 'SConscript'),
                       variant_dir='build/components/rtgui',
                       duplicate=0)
else:
    objs = PrepareBuilding(env, RTT_ROOT, has_libcpu=False)

# build program
program = env.Program(TARGET, objs)

# end building
EndBuilding(TARGET, program)
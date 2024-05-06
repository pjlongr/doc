# avlink方案内存分配

- struct scene_s memrgn_info[] 定义不同模块的内存信息

	路径：

	platform\memrgn.inc

	include\platform\mem_cfg.inc

	```C
	//内存模式的枚举
	enum MEMRGN_MODE {
		SCN_FREE = 0,
		MEMMAP_MODE_LIVE = 1,
		MEMMAP_MODE_FILE = 2,
		MEMMAP_MODE_PICTURE = 3,
		MEMMAP_MODE_THUMBNAIL = 4,
		MEMMAP_MODE_SMART_UPGRADE = 5,
		MEMMAP_MODE_NET_PARSE = 6,
		MEMMAP_MODE_NET_PLAY = 7,
		MEMMAP_MODE_NET_PLAY_EXT = 8,
		MEMMAP_MODE_NET_PICTURE = 9,
		MEMMAP_MODE_GPU_LIVE = 10,
	};
	struct scene_s {
		struct memrgn_s *memrgn;
		unsigned char state;
	};
	struct scene_s memrgn_info[] = {
		{ memrgn_free, 1 },
		{ memrgn_MEMMAP_MODE_LIVE, 0 },
		{ memrgn_MEMMAP_MODE_FILE, 0 },
		{ memrgn_MEMMAP_MODE_PICTURE, 0 },
		{ memrgn_MEMMAP_MODE_THUMBNAIL, 0 },
		{ memrgn_MEMMAP_MODE_SMART_UPGRADE, 0 },
		{ memrgn_MEMMAP_MODE_NET_PARSE, 0 },
		{ memrgn_MEMMAP_MODE_NET_PLAY, 0 },
		{ memrgn_MEMMAP_MODE_NET_PLAY_EXT, 0 },
		{ memrgn_MEMMAP_MODE_NET_PICTURE, 0 },
		{ memrgn_MEMMAP_MODE_GPU_LIVE, 0 },
	};
	```

- MCF_SwitchMode （platform\memrgn.inc）

	切换内存模式的函数

- Get_MCF_SwitchMode （platform\mem_cfg.c）

	获取当前是哪种内存模式


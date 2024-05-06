### Authentication

```
player_api.php?username=X&password=X
```

### Get Live [Stream](https://so.csdn.net/so/search?q=Stream&spm=1001.2101.3001.7020) Categories

```
player_api.php?username=X&password=X&action=get_live_categories
```

### Get VOD Stream Categories

```
player_api.php?username=X&password=X&action=get_vod_categories
```

### Get SERIES Stream Categories

```
player_api.php?username=X&password=X&action=get_series_categories
```

### Get Live Streams

```
player_api.php?username=X&password=X&action=get_live_streams  (this will get all live streams)
player_api.php?username=X&password=X&action=get_live_streams&category_id=X  (this will get all live streeams  in the selected category only)
```

### Get VOD Streams

```
player_api.php?username=X&password=X&action=get_vod_streams  (this will get all  vod streams)
player_api.php?username=X&password=X&action=get_vod_streams&category_id=X  (this will get all vodstreeams  in the selected category only)
```

### Get Series Streams

```
player_api.php?username=X&password=X&action=get_series  (this will get all series streams)
player_api.php?username=X&password=X&action=get_series&category_id=X  (this will get all series streeams  in the selected category only)
```

### Get series info

```
player_api.php?username=X&password=X&action=get_series_info&series_id=x
```

### Get Vod info

```
player_api.php?username=X&password=X&action=get_vod_info&vod_id=x
```

### Get short_epg for live streams (same as stalker portal,prints the next X EPG that will play soon)

```
player_api.php?username=X&password=X&action=get_short_epg&stream_id=X
player_api.php?username=X&password=X&action=get_short_epg&stream_id=X&linmit=X  (You can specify a linmit too,without limit the default)
```

### Get all epg for Live Streams (same as stalker portal,but it will printf all epg listings regardless of the day)

```
player_api.php?username=X&password=X&action=get_simple_date_table&stream_id=X
```

### Full Epg List for all Streams

```
xmltv.php?username=&password=X
```

### Play URL

```
{host:port}/{stream_type}/{user}/{password}/{stream_id}.{container_extension}
```


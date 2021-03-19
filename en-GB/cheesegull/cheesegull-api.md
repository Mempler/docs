FORMAT: 1A
HOST: https://api.chimu.moe

# Chimu API
NOTE: Some API Endpoints need a special API key which are only for patreon or given away  \
      by @Mempler#6666 on [Discord](https://discord.gg/9JnqPSrPpz) \
      or via E-Mail at [chimu@mempler.de](mailto:chimu@mempler.de)!

## API V1
### Get beatmap [GET /v1/map/{map_id}]
Gets a beatmap.
if it fails it returns a simple JSON Serialized error including
a message and error code.

+ Parameters

    + map_id: `53` (required, number) - Beatmap Id

+ Response 200 (application/json)

    + Body
    
            {
                "code": 0,
                "message": "",
                "data": {
                    "BeatmapId": 53,
                    "ParentSetId": 3,
                    "DiffName": "-Crusin-",
                    "FileMD5": "1d23c37a2fda439be752ae2bca06c0cd",
                    "Mode": 0,
                    "BPM": 172.0,
                    "AR": 4.0,
                    "OD": 4.0,
                    "CS": 5.0,
                    "HP": 3.0,
                    "TotalLength": 83,
                    "HitLength": 77,
                    "Playcount": 101574,
                    "Passcount": 47994,
                    "MaxCombo": 124,
                    "DifficultyRating": 1.86918,
                    "OsuFile": "Ni-Ni - 1,2,3,4, 007 [Wipeout Series] (MCXD) [-Crusin-].osu",
                    "DownloadPath": "/d/3"
                }
            }

+ Response 404 (application/json)

    + Body

            {
                "code": 104,
                "message": "Error: Beatmap not found!",
                "data": []
            }

### Get beatmap set [GET /v1/set/{set_id}]
Gets a beatmap set.
if it fails it returns a simple JSON Serialized error including
a message and error code.

+ Parameters

    + set_id: `1` (required, number) - Beatmap set Id

+ Response 200 (application/json)

    + Body
    
            {
                "code": 0,
                "message": "",
                "data": {
                    "SetId": 1,
                    "ChildrenBeatmaps": [
                    {
                        "BeatmapId": 75,
                        "ParentSetId": 1,
                        "DiffName": "Normal",
                        "FileMD5": "a5b99395a42bd55bc5eb1d2411cbdf8b",
                        "Mode": 0,
                        "BPM": 119.999,
                        "AR": 6.0,
                        "OD": 6.0,
                        "CS": 4.0,
                        "HP": 6.0,
                        "TotalLength": 142,
                        "HitLength": 109,
                        "Playcount": 467474,
                        "Passcount": 57119,
                        "MaxCombo": 314,
                        "DifficultyRating": 2.40729,
                        "OsuFile": "Kenji Ninuma - DISCO PRINCE (peppy) [Normal].osu",
                        "DownloadPath": "/d/1"
                    }
                    ],
                    "RankedStatus": 1,
                    "ApprovedDate": "2007-10-06T17:46:31",
                    "LastUpdate": "2007-10-06T17:46:31",
                    "LastChecked": "2021-01-19T09:43:57",
                    "Artist": "Kenji Ninuma",
                    "Title": "DISCO PRINCE",
                    "Creator": "peppy",
                    "Source": "",
                    "Tags": "katamari",
                    "HasVideo": false,
                    "Genre": 2,
                    "Language": 3,
                    "Favourites": 789,
                    "Disabled": false
                }
            }

+ Response 404 (application/json)

    + Body

            {
                "code": 104,
                "message": "Error: Beatmap set not found!",
                "data": []
            }

### Download beatmap set [GET /v1/download/{set_id}{?k,s,n}]
Downloads a beatmap set using given set id

+ Parameters

    + set_id: `1` (required, number) - Beatmap set Id
    + n: `1` (required, string) - Download either with or without video.

+ Response 302

    + header

            https://ipfs.chimu.moe/ipfs/QmbrVpStvVtznG5p8u5HebThepUaGgK7dmRbeUTXqrBEvK?filename=1 Kenji Ninuma - DISCO PRINCE.osz

+ Response 403 (application/json)

    + Body

            // if {k} is not set
            {
                "code": 102,
                "message": "Error: key is not set!",
                "data": []
            }

            // if {s} is not set
            {
                "code": 103,
                "message": "Error: state is not set!",
                "data": []
            }

            // if {k} is invalid
            {
                "code": 102,
                "message": "Error: Invalid Key!",
                "data": []
            }

+ Response 401 (application/json)

    + Body

            // if {set_id} is not an int set
            {
                "code": 101,
                "message": "Error: ABVC is not an int!",
                "data": []
            }

+ Response 404 (application/json)

    + Body

            // if we don't know about this beatmap.
            {
                "code": 104,
                "message": "Error: Beatmap not found!",
                "data": []
            }
            
            // if we dont have this beatmap and cant download it.
            {
                "code": 105,
                "message": "Error: Beatmap unavailable!",
                "data": []
            }

### Search [GET /v1/search{?query,amount,offset,status,mode,min_ar,max_ar,min_od,max_od,min_cs,max_cs,min_hp,max_hp,min_diff,max_diff,min_bpm,max_bpm,min_length,max_length,genre,language}]
Search for a beatmap set

+ Parameters
    
    + query: `peppy` (string) - Search query
    + amount: `100` (number) - Amount of resulting beatmaps
    + offset: `0` (number) - Offset E.G amount + 100 would result in the next page
    + status: `0` (number) - Ranked status of the Beatmap Set
    + mode: `0` (number) - Required playmode of the beatmap
    + min_ar: `0` (number) - Minimum AR
    + max_ar: `0` (number) - Maximum AR
    + min_od: `0` (number) - Minimum OD
    + max_od: `0` (number) - Maximum OD
    + min_cs: `0` (number) - Minimum CS
    + max_cs: `0` (number) - Maximum CS
    + min_hp: `0` (number) - Minimum HP
    + max_hp: `0` (number) - Maximum HP
    + min_diff: `0` (number) - Minimum Difficulty
    + max_diff: `0` (number) - Maximum Difficulty
    + min_bpm: `0` (number) - Minimum BPM
    + max_bpm: `0` (number) - Maximum BPM
    + min_length: `0` (number) - Minimum Beatmap length (in seconds)
    + max_length: `0` (number) - Maximum Beatmap length (in seconds)
    + genre: `0` (number) - Genre
    + language: `0` (number) - Language

+ Response 401 (application/json)

    + Body

            {
                "code": 101,
                "message": "Error: ABCD is not an int!",
                "data": []
            }

### Cheesegull> Get beatmap [GET /cheesegull/b/{map_id}]
Gets a beatmap.

+ Parameters

    + map_id: `53` (required, number) - Beatmap Id

+ Response 200 (application/json)

    + Body
    
            {
                  "BeatmapID": 53,
                  "ParentSetID": 3,
                  "DiffName": "-Crusin-",
                  "FileMD5": "1d23c37a2fda439be752ae2bca06c0cd",
                  "Mode": 0,
                  "BPM": 172,
                  "AR": 4,
                  "OD": 4,
                  "CS": 5,
                  "HP": 3,
                  "TotalLength": 83,
                  "HitLength": 77,
                  "Playcount": 101574,
                  "Passcount": 47994,
                  "MaxCombo": 124,
                  "DifficultyRating": 1.86918
            }

### Cheesegull> Get beatmapset [GET /cheesegull/s/{set_id}]
Gets a beatmap set.

+ Parameters

    + set_id: `1` (required, number) - Beatmapset Id

+ Response 200 (application/json)

    + Body
    
            {
                  "SetID": 1,
                  "ChildrenBeatmaps": [
                        {
                              "BeatmapID": 75,
                              "ParentSetID": 1,
                              "DiffName": "Normal",
                              "FileMD5": "a5b99395a42bd55bc5eb1d2411cbdf8b",
                              "Mode": 0,
                              "BPM": 119.999,
                              "AR": 6,
                              "OD": 6,
                              "CS": 4,
                              "HP": 6,
                              "TotalLength": 142,
                              "HitLength": 109,
                              "Playcount": 467474,
                              "Passcount": 57119,
                              "MaxCombo": 314,
                              "DifficultyRating": 2.40729
                        }
                  ],
                  "RankedStatus": 1,
                  "ApprovedDate": "2007-10-06T17:46:31Z",
                  "LastUpdate": "2007-10-06T17:46:31Z",
                  "LastChecked": "2021-01-19T09:43:57Z",
                  "Artist": "Kenji Ninuma",
                  "Title": "DISCO PRINCE",
                  "Creator": "peppy",
                  "Source": "",
                  "Tags": "katamari",
                  "HasVideo": false,
                  "Genre": 2,
                  "Language": 3,
                  "Favourites": 789
            }
            
### Cheesegull> Search [GET /cheesegull/search{?query,amount,offset,status,mode}]
Search for a beatmap set

+ Parameters

    + query: `peppy disco` (string) - Search query
    + amount: `1` (number) - Amount of resulting beatmaps
    + offset: `0` (number) - Offset E.G amount + 100 would result in the next page
    + status: `0` (number) - Ranked status of the Beatmap Set
    + mode: `0` (number) - Required playmode of the beatmap

+ Response 200 (application/json)

    + Body

            [
                  {
                        "SetID": 1,
                        "ChildrenBeatmaps": [
                              {
                                    "BeatmapID": 75,
                                    "ParentSetID": 1,
                                    "DiffName": "Normal",
                                    "FileMD5": "a5b99395a42bd55bc5eb1d2411cbdf8b",
                                    "Mode": 0,
                                    "BPM": 119.999,
                                    "AR": 6,
                                    "OD": 6,
                                    "CS": 4,
                                    "HP": 6,
                                    "TotalLength": 142,
                                    "HitLength": 109,
                                    "Playcount": 467474,
                                    "Passcount": 57119,
                                    "MaxCombo": 314,
                                    "DifficultyRating": 2.40729
                              }
                        ],
                        "RankedStatus": 1,
                        "ApprovedDate": "2007-10-06T17:46:31Z",
                        "LastUpdate": "2007-10-06T17:46:31Z",
                        "LastChecked": "2021-01-19T09:43:57Z",
                        "Artist": "Kenji Ninuma",
                        "Title": "DISCO PRINCE",
                        "Creator": "peppy",
                        "Source": "",
                        "Tags": "katamari",
                        "HasVideo": false,
                        "Genre": 2,
                        "Language": 3,
                        "Favourites": 789
                  }
            ]

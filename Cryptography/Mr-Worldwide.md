# Cryptography
## Mr-Worldwide - 200 Points
## Question
>A musician left us a message. What's it mean?

## Solution

> The message shows
`[K]yoto             (35.028309, 135.753082)
[O]dessa            (46.469391, 30.740883)
[D]ayton            (39.758949, -84.191605)
[I]stanbul          (41.015137, 28.979530)
[A]bu Dhabi         (24.466667, 54.366669)
[K]uala Lumpur      (3.140853, 101.693207)
_
[A]ddis Ababa       (9.005401, 38.763611)
[L]oja              (-3.989038, -79.203560)
[A]msterdam         (52.377956, 4.897070)
[S]leepy Hollow     (41.085651, -73.858467)
[K]odiak            (57.790001, -152.407227)
[A]lexandria        (31.205753, 29.924526)`

`
> Sometimes the location is a city, sometimes a region. Due to this reason, it's hard to solve the challenge programmatically. The closest I got is by using the geocode field of a geocoding service, via the following script:

```python
import requests, re

flag = "picoCTF{(35.028309, 135.753082)(46.469391, 30.740883)(39.758949, -84.191605)(41.015137, 28.979530)(24.466667, 54.366669)(3.140853, 101.693207)_(9.005401, 38.763611)(-3.989038, -79.203560)(52.377956, 4.897070)(41.085651, -73.858467)(57.790001, -152.407227)(31.205753, 29.924526)}"

def get_letter_from_coordinate(match):
    lat = match.group(1)
    long = match.group(2)
    r = requests.get("https://geocode.xyz/{},{}?json=1".format(lat, long))
    j = r.json()
    return j["geocode"][0]

print re.sub(r'\(([\d\.-]+), ([\d\.-]+)\)', get_letter_from_coordinate, flag)
```

> Flag is: picoCTF{KODIAK_ALANKA}
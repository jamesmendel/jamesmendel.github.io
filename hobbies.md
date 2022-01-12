---
layout: frontpage
title: Hobbies
---

# Hobbies

I have many hobbies. You'll notice that most of them revolve around technology.
An incomprehensive list:
- Mechanical keyboards
- Music (listening)
- 3D Printing
- Programming
- Creative coding (interactive design)
- Web design
- Creating

### My current keyboard:
[(Picture)]({{ site.baseurl }}/images/Keyboard.jpg){:target="_blank"}

| Component | Model                     |
|-----------|---------------------------|
| Case      | Massdrop Alt High Profile |
| Switches  | Gateron Black Inks        |
| Lubricant | Krytox G205 Grade 0       |
| Keycaps   | GMK Taro R2               |


### My current desktop:

| Component    | Model                            |
|--------------|----------------------------------|
| Processor    | Intel i5-7600K @3.8GHz           |
| Memory       | 16GB DDR4                        |
| Graphics     | NVIDIA GeForce GTX 1080          |
| HDD 1 (Boot) | 240GB Kingston SSD               |
| HDD 2, 3     | 2x 1TB WD Blue (RAID 0)          |

### Music:

My current peripherals

| Component  | Model                   |
|------------|-------------------------|
| Headphones | Drop x Senheiser HD 6XX |
| Headphones | Audio Technica ATH-M40x |
| Earbuds    | Apple Air Pods Pro      |
| Receiver   | Yamaha CRX E-300        |
| Speakers   | Micca MB42X             |

Some of my favorite albums

| Artist          | Album           |
|-----------------|-----------------|
| The Beatles     | Revolver        |
| Tame Impala     | Inner Speaker   |
| Parcels         | Parcels         |
| Fleetwood Mac   | Rumours         |
| LCD Soundsystem | Sound of Silver |

Or you can hear what I'm listening to right now on [Spotify](https://open.spotify.com/user/12187197787?si=6c2dbcb003764445){:target="_blank"}!


{% for post in site.categories.gear %}

<a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>

{{ post.date | date_to_string }}

<p>{{ post.excerpt }}</p>

<p><a href="{{ site.baseurl }}{{ post.url }}">More...</a></p>

-----

{% endfor %}

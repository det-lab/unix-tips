# Creating Example Files
Now that we've gotten some of the more vital commands down, let's use this as an opportunity to start creating some files that we can test our command knowledge on. This section will show the explicit creation of the example files, but you can also simply download the three created files from here and move them to a chosen directory:

* [planets.md](examples/planets.md)

* [constellations.md](examples/constellations.md)

* [galaxies.md](/examples/galaxies.md)

To create the files, first, navigate to a folder that you'd be willing to use to house some files for this lesson by using the `cd` command, such as:
```bash
cd ~
```
To navigate to the home directory. From there, create a new folder which can be used to house your example files:
```bash
mkdir Unix-Tips
```
You'll then want to enter this directory using:
```bash
cd Unix-Tips
```
Now, let's create and edit some files using the `touch` command and a text editor of your choice. This lesson will be using `nano`. We'll want to create some files that have several lines with distinct words or phrases for the later sections which will focus on editing and searching. For this example, we'll be creating a couple of files with astronomical data:
```bash
touch planets.md
touch constellations.md
touch galaxies.md
```
Let's start by creating our `planets.md` file. We'll create one line for each planet and include its radius, distance from the sun, and mass. Start by entering:
```bash
nano planets.md
```
Then, inside the file, we can add our information:

| Planet | Radius (m) | Distance (AU) | Mass (Kg) |
| ---    | ---        | ---           | ---       |
| Mercury| 2.44E6     | 0.39          | 3.29E23   |
| Venus  | 6.05E6     | 0.72          | 4.87E24   |
| Earth  | 6.37E6     | 1             | 5.97E24   |
| Mars   | 3.39E6     | 1.52          | 6.42E23   |
| Jupiter| 7.15E7     | 5.2           | 1.90E27   |
| Saturn | 6.03E7     | 9.54          | 5.69E26   |
| Uranus | 2.56E7     | 19.2          | 8.68E25   |
| Neptune| 2.48E7     | 30.1          | 1.02E26   |

Then save and exit the file using `CTRL+X` and `Y` before creating data for the second two files:
```bash
nano constellations.md
```
With data from the Zodiac constellations:

| Constellation | Brightest Star |
| ---           | ---            |
| Aries         | Hamal          |
| Taurus        | Aldebaran      |
| Gemini        | Pollux         |
| Cancer        | Al Tarf        |
| Leo           | Regulus        |
| Virgo         | Spica          |
| Libra         | Zubeneschamali |
| Scorpius      | Antares        |
| Ophiuchus     | Rasalhague     |
| Sagittarius   | Kaus Australis |
| Capricornus   | Deneb Algedi   |
| Aquarius      | Sadalsuud      |
| Pisces        | Alpherg        |

And finally, let's fill galaxies.md with a small selection of galaxies in our local group:
```bash
nano galaxies.md
```
| Galaxy             | Constellation | Type      |
| ---                | ---           | ---       |
| Andromeda          | Andromeda     | Spiral    |
| Milky Way (Center) | Sagittarius   | Spiral    |
| Triangulum         | Triangulum    | Spiral    |
| Pisces Dwarf       | Pisces        | Irregular |
| Leo A              | Leo           | Irregular |
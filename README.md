# se-simulator
Generating fun Stack Exchange questions using Markov chains

### [try it out](http://se-simulator.lw1.at/)


### Setup

- `pip install -r requirements.txt`
- create a MySQL database called `se-simulator`
- rename `config.sample.py` to `config.py` and fill in the database details and create a `secret_key`
- run `create.py`, which creates the database and fetches the list of SE sites
- run `apply_colors.py` (which should run really quickly)
- create folders called `chains`, `download` and `raw` (or syminks to somewhere where more disk space is left)
- download the `.7z` files for the sites you want to generate (I'd recommend to use a file <100MB)
    - If the `.7z` has another name as the site has now, rename it
- run `consume.py`
    - It should check the hash, move the file to `raw/`, unpack it and extract the needed content from the `.xml` files into new `.jsonl` files. It also writes the data of the file into the db, so it won't be imported again.
- now the most important step: run `todb.py`
    - this will generate the markov chains and save them (or use existing ones on the next run)
    - afterwards 100 questions will be added to the db, with corresponding answers, titles and usernames
- run `shuffle.py`
    - I haven't found a performant way to get a random question without asigning every question an integer and saving the maximum to `count.txt`
- run `server.py`
    - this starts the Flask server on `http://127.0.0.1:5000/`
    - if I didn't miss an important step, the site should be working fine now.
    
### other files

- `app.py`: needed for Flask
- `basemodel.py` and `models.py`: [peewee](https://github.com/coleifer/peewee/) ORM
- `extra_data.py`: manually collected colors of every site with an custom theme
- `markov.py`: extending the great [markovify library](https://github.com/jsvine/markovify/) for my use case
- `parsexml.py`: reading in the Stack Exchange dump XML files with no more than 40MB RAM usage.
- `text_generator.py`: everything that creates the content and handles the Markov chains
- `updater.py`: probably not working anymore, checks for newer dump files
- `utils.py`: everything else

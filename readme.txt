https://docs.python.org/3/tutorial/venv.html

mkdir ./cicd
cd cicd
mkdir buzz
mkdir tests
python3 -m venv venv
source ./venv/bin/activate
echo "pytest==3.0.6" > requirements.txt
pip install -r requirements.txt
python3 -m pytest -v tests/test_generator.py
echo "venv" > .gitignore
git init
git add *
git commit -m "Initial commit"

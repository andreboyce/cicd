#https://docs.python.org/3/tutorial/venv.html

mkdir ./cicd
cd cicd
mkdir buzz
mkdir tests
python3 -m venv venv
source ./venv/bin/activate
echo "pytest==3.0.6" > requirements.txt
echo "# cicd" >> README.md
pip install -r requirements.txt
python3 -m pytest -v tests/test_generator.py
echo "venv" > .gitignore
git init
git add *
git commit -m "Initial commit"
git remote add origin https://github.com/andreboyce/cicd.git
#git remote set-url origin https://github.com/andreboyce/cicd.git
git push -u origin master
#Connect Travis CI to run the tests on every commit
echo "language: python" >> .travis.yml
echo "script:" >> .travis.yml
echo "  - python -m pytest -v" >> .travis.yml
git add .travis.yml
git commit -m "Add Travis CI configuration"
git push
git commit -m "Updated readme.txt"
git push
touch app.py
echo "Flask==0.12" >> requirements.txt
pip install -r requirements.txt
#python app.py
git commit -m "Added App.py wrapper"
git push

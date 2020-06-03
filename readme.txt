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
echo "Flask==1.0" >> requirements.txt
pip install -r requirements.txt
#python app.py
git add app.py
git add requirements.txt
git add readme.txt
git commit -m "Added App.py wrapper"
git push
#
touch Dockerfile
docker build -t cicd .
#docker run -p 5000:5000 --rm -it cicd
#
git add Dockerfile
git commit -m "Dockerized"
git push
#
mkdir .travis
touch ./.travis/deploy_dockerhub.sh
git add ./.travis/deploy_dockerhub.sh
#on travis-ci.org DOCKER_EMAIL
#on travis-ci.org DOCKER_PASS
#on travis-ci.org DOCKER_USER
git commit -m "Travis CI push to dockerhub"
git push
git add -A
git push
#
docker run -p5000:5000 --rm -it andreboyce/cicd:latest
#

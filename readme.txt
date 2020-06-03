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
DOCKER_USER=
DOCKER_PASS=
docker build -t $DOCKER_USER/cicd .
#docker build -f Dockerfile -t $DOCKER_USER/cicd:latest .
#docker login -u $DOCKER_USER -p $DOCKER_PASS
#docker push $DOCKER_USER/cicd:latest

#docker run -p 5000:5000 --rm -it $DOCKER_USER/cicd
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
heroku login
heroku create
heroku plugins:install @heroku-cli/plugin-container-registry
heroku container:login
heroku container:push web
heroku container:release web
heroku open
#
#HEROKU_API_KEY 
#HEROKU_APP_NAME 
echo "  - test \"\$TRAVIS_BRANCH\" = \"master\" && sh .travis/deploy_heroku.sh" >> .travis.yml
touch ./.travis/deploy_heroku.sh
echo '#!/bin/sh'  >> ./.travis/deploy_heroku.sh
echo "wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh" >> ./.travis/deploy_heroku.sh
echo "heroku container:login" >> ./.travis/deploy_heroku.sh
echo "heroku container:push web --app \$HEROKU_APP_NAME" >> ./.travis/deploy_heroku.sh
echo "heroku container:release web --app \$HEROKU_APP_NAME" >> ./.travis/deploy_heroku.sh

git add ./.travis/deploy_heroku.sh
git add .travis.yml
git add readme.txt
git add -A
git commit -m "Adding Heroku Push to Travis CI"
git push

rm -rf docker-devops || echo "First run"
git clone https://github.com/AdityaSP/docker-devops
cd docker-devops
docker image build -t <dockerusername>/sept20app:$BUILD_NUMBER .
docker login -u <dockerusername> -p <dockerhubpassword>
docker image push <dockerusername>/sept20app:$BUILD_NUMBER
#DOCKERUSER/IMAGENAME:BUILDNUMBER
sed -i "s/DOCKERUSER/<dockerusername>/" dep.yml
sed -i "s/IMAGENAME/sept20app/" dep.yml
sed -i "s/BUILDNUMBER/${BUILD_NUMBER}/" dep.yml
kubectl apply -f dep.yml

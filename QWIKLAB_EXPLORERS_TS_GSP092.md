# Monitoring and Logging for Cloud Functions || [GSP092](https://www.cloudskillsboost.google/focuses/1833?parent=catalog) ||

# # Like, comment, share & Don't forget to subscribe [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN) 👍😄🤝

### Run the following Commands in CloudShell

```
export REGION=
```
```
gcloud services enable \
  artifactregistry.googleapis.com \
  cloudfunctions.googleapis.com \
  cloudbuild.googleapis.com \
  eventarc.googleapis.com \
  run.googleapis.com \
  logging.googleapis.com \
  pubsub.googleapis.com

mkdir ~/hello-http && cd $_
touch index.js && touch package.json

cat > index.js <<EOF
/**
 * Responds to any HTTP request.
 *
 * @param {!express:Request} req HTTP request context.
 * @param {!express:Response} res HTTP response context.
 */
exports.helloWorld = (req, res) => {
    let message = req.query.message || req.body.message || 'Hello World!';
    res.status(200).send(message);
  };
  
EOF

cat > package.json <<EOF
{
    "name": "sample-http",
    "version": "0.0.1"
  }
  
EOF



deploy_function() {
  gcloud functions deploy helloWorld \
--trigger-http \
--runtime nodejs20 \
--allow-unauthenticated \
--region $REGION \
--max-instances 5
}

deploy_success=false

while [ "$deploy_success" = false ]; do
  if deploy_function; then
    echo "Cloud Run service is created. Exiting the loop."
    deploy_success=true
  else
    echo "Waiting for Cloud Run service to be created..."
    sleep 60
  fi
done

echo "Running the next code..."

curl -LO 'https://github.com/tsenart/vegeta/releases/download/v6.3.0/vegeta-v6.3.0-linux-386.tar.gz'

tar xvzf vegeta-v6.3.0-linux-386.tar.gz

gcloud logging metrics create CloudFunctionLatency-Logs \
    --project=$DEVSHELL_PROJECT_ID \
    --description="awesome lab" \
    --log-filter='resource.type="cloud_function" AND resource.labels.function_name="helloWorld" AND log_name="projects/$DEVSHELL_PROJECT_ID/logs/cloudaudit.googleapis.com%2Factivity" AND resource.labels.region="$REGION"'
```

# Congratulations ..!!🎉  You completed the lab shortly..😃💯

# *Well done..!* 👏

# Thank you for visiting.... :) 🗯️

# [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN)

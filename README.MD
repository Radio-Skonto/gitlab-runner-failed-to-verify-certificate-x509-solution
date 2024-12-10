I have figured out the solution:

Use openssl command to fetch the certificate directly from the GitLab server and save it to a file:

openssl s_client -showcerts -connect <git_url>:443 </dev/null 2>/dev/null|openssl x509 -outform PEM > gitlab.crt
View the received certificate (this returns a big blob of data):

openssl x509 -in gitlab.crt -text -noout
Add GitLab Server Certificate to Trusted Certificates:

sudo mkdir -p /usr/local/share/ca-certificates/
sudo cp gitlab.crt /usr/local/share/ca-certificates/
sudo update-ca-certificates
Retry GitLab Runner Registration:

sudo gitlab-runner register
Follow through with the prompts and now I don’t receive the tls error:

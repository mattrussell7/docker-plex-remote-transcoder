FROM mattflix-gpu:base                                                                                    
                                                                                                          
# Set environment variables                                                                               
ENV S6_BEHAVIOUR_IF_STAGE2_FAILS=2                                                                        
                                                                                                          
RUN \                                                                                                     
# Install packages                                                                                        
        buildDeps=" \                                                                                     
                curl \                                                                                    
                unzip" && \                                                                               
        apt-get update && \                                                                               
        apt-get upgrade -y && \                                                                           
        apt-get install -y --no-install-recommends --no-install-suggests \                                
                ${buildDeps} \                                                                            
                ca-certificates \                                                                         
                inotify-tools \                                                                           
                jq \                                                                                      
                nfs-kernel-server \                                                                       
                openssh-client \                                                                          
                openssh-server \                                                                          
                python-psutil \                                                                           
                python-setuptools && \                                                                    
# Add Plex-Remote-Transcoder                                                                              
        curl -o /tmp/prt-testing.zip -L "https://github.com/mattrussell7/Plex-Remote-Transcoder/archive/master.zip
ster.zip" && \                                                                                            
        mkdir -p /app && \                                                                                
        unzip /tmp/prt-testing.zip -d /app/prt-testing && \                                               
# Configure Plex-Remote-Transcoder                                                                        
        mkdir -p /var/lib/plexmediaserver && \                                                            
        ln -s /config/Library /var/lib/plexmediaserver/Library && \                                       
# Configure NFS Server                                                                                    
        echo "nfs             2049/tcp" >> /etc/services && \                                             
        mkdir -p \                                                                                        
                /exports/plex_bin.d \                                                                     
                /exports/plex_conf.d \                                                                    
                /exports/plex_media.d \                                                                   
                /exports/plex_transcode.d && \                                                            
# Configure SSHD                                                                                          
        mkdir /var/run/sshd && \                                                                          
        printf "StrictHostKeyChecking no\n" >> /etc/ssh/ssh_config && \                                   
# Configure user                                                                                          
        sed -i -e 's;/config:/bin/false;/config:/bin/bash;g' /etc/passwd && \                             
# Clean                                                                                                   
        rm -rf \                                                                                          
                /tmp/* \                                                                                  
                /var/lib/apt/lists/* \                                                                    
                /var/tmp/*                                                                                
                                                                                                          
COPY root/ /                                                                                              
VOLUME /data /usr/lib/plexmediaserver /exports                                                            
EXPOSE 22 2049/tcp                                                                                        

FROM fastai/jekyll:2021-02-11

WORKDIR /fastpages
COPY . .
RUN chmod u+x action_entrypoint.sh
RUN chmod u+x word2post.sh
RUN dos2unix /fastpages/*.sh

CMD [ "/fastpages/action_entrypoint.sh" ]

<template>
    <b-container class="main movie">
        <b-row>
            <b-col><img class="img-filme" :src="dataMovie.large_cover_image"></b-col>
            <b-col class="metaDados">
                <h2>{{ dataMovie.title }}</h2>
                <div>
                    <span>{{ dataMovie.year }}</span> <span>{{ hours }} horas e {{ minutes }} minutos</span>
                </div>
                <a :href="'https://www.imdb.com/title/' + dataMovie.imdb_code">
                    <div>
                        <img src="../assets/logo-imdb.svg"> {{ dataMovie.rating }}
                    </div>
                </a>
                <button @click="downloadTorrent" class="watch">Watch</button>
                <div class="descricao">
                    <p>{{ dataMovie.description_full }}</p>
                </div>
            </b-col>
            <b-col>
                <div class="files"></div>
                <p>Diretiva v-html: <span v-html="arq"></span></p>
            </b-col>
        </b-row>
    </b-container>
</template>

<script>
import axios from 'axios';
import WebTorrent from 'webtorrent/dist/webtorrent.min.js'

var client = new WebTorrent()
export default {
    name: 'VideoPlayer',
    data() {
        return {
            idMovie: this.$route.params.id,
            dataMovie: [],
            arq: "123",
            idTorrent: "",
            magnetURI: "https://yts.mx/torrent/download/3924C0D622FB1ED5E0D6F6CCDBF8529FB7916E2B",
        };
    },
    created() {
        this.getMovie();
        this.checkRoute();
    },
    mounted() {
        
    },
    methods: {
        async getMovie() {
            axios.get('https://yts.mx/api/v2/movie_details.json?movie_id=' + this.idMovie)
                .then(getMetaMovie => {
                    console.log(getMetaMovie.data);
                    this.dataMovie = getMetaMovie.data.data.movie;
                    this.idTorrent = getMetaMovie.data.data.movie.torrents[0].url;

                })
                .catch(error => {
                    console.log(error)
                    console.log("nao foi")
                })
        },
        downloadTorrent() {
            const torrentId = 'https://yts.mx/torrent/download/3924C0D622FB1ED5E0D6F6CCDBF8529FB7916E2B';
            client.add(torrentId, (torrent) => {
                console.log('Client is downloading:', torrent.infoHash);
                const file = torrent.files.find(function (file) {
                    return file.name.endsWith('.mp4')
                })
                file.renderTo('.files')
            });
        },
        watch() {
            alert('12');
        },

    },
    computed: {
        hours() {
            return Math.floor(this.dataMovie.runtime / 60);
        },
        minutes() {
            return this.dataMovie.runtime % 60;
        }
    }
}

</script>
<style>
.watch {
    background-color: #f3b52f;
    color: black;
    margin-top: 30px;
    border: none;
    border-radius: 10px;
    padding: 10px 20px 10px 20px;
}

.descricao {
    margin-top: 30px;
    margin-bottom: 30px;
}

.movie {
    color: white;
}

.metaDados {
    text-align: left;
}

.img-filme {
    width: 300px;
    object-fit: contain;
    border-radius: 20px;
}
</style>
<template>
    <div id="app" v-show="responseTexts">
        <div class="preview" v-for="(item, key) in reviewJson"
             :key="key">
            <img class="preview__iframe"
                 v-show="responseTexts[key]"
                 v-bind:src="responseTexts[key]"
                 @click="chooseReview = key"/>
            <div class="error"
                 v-if="imageErrors[key] && imageErrors[key].status">
                <strong class="error">
                    Error status {{imageErrors[key].status }}
                </strong>
            </div>
            <ul class="list"
                v-if="chooseReview === key">
                <li class="list__item">{{item.description1}}</li>
                <li class="list__item">{{item.description2}}</li>
            </ul>
            <p class="preview__name">{{item.name}}</p>
        </div>
    </div>
</template>

<script>

	export default {
		data () {
			return {
				chooseReview: undefined,
				responseTexts: [],
				imageErrors: [],
				reviewJson: [
					{
						url: 'http://google.com',
						name: 'Google',
						description1: 'Lorem ipsum dolor sit amet.',
						description2: 'lorem2 lorem2 lorem2 lorem2'
					},
					{
						url: 'http://yahoo.com',
						name: 'Yahoo',
						description1: 'Lorem ipsum dolor sit amet.',
						description2: 'lorem2 lorem2 lorem2 lorem2'
					},
					{
						url: 'http://bing.com',
						name: 'Bing',
						description1: 'Lorem ipsum dolor sit amet.',
						description2: 'lorem2 lorem2 lorem2 lorem2'
					},
					{
						url: 'http://example.com',
						name: 'Example',
						description1: 'Lorem ipsum dolor sit amet.',
						description2: 'lorem2 lorem2 lorem2 lorem2'
					}
                ]
			}
		},
		methods: {
			fetchData(site, index) {
				this.$http.get('https://mini.s-shot.ru/1024x768/JPEG/1024/Z100/?' + site)
					.then((data) => {
						this.responseTexts[index] = data.url;
						this.responseTexts.push('').pop();
					}).catch((err) => {
					if (err) {
						this.imageErrors[index] = err;
						this.imageErrors.push('');
						this.imageErrors.pop();
					}
				});
			}
		},
		created() {
			this.reviewJson.forEach((item, i) => {
				this.fetchData(item.url, i);
			})
		}
	}

</script>

<style lang="scss">
    * {
        margin: 0;
        padding: 0;
    }

    #app {
        text-align: center;
        background-color: #000
    }
    .error {
        font-family: Arial, Calibri;
        font-size: 22px;
        font-weight: 900;
        color: red;
        margin-bottom: 40px;
    }
    .preview {
        width: 800px;
        margin: 0 auto;
        padding-bottom: 50px;
        &__iframe {
            width: 100%;
            height: 400px;
            margin-bottom: 50px
        }
        &__name {
            font-family: Arial, Calibri;
            font-size: 22px;
            font-weight: 900;
            color: #fff;
            cursor: pointer;
        }
    }
    .list {
        width: 100%;
        text-align: left;
        margin-bottom: 30px;
        &__item {
            color: #fff;
            list-style-type: none;
            margin: 10px 0;
        }
    }
</style>
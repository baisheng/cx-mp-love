<template>
  <div class="container">
    <div>
      <h3>Welcome to React + GraphQL + Apollo minimal boilerplate</h3>
      <card :text="user.id"></card>
      <card :text="user.name"></card>
    </div>
  </div>
</template>

<script>
  import card from '@/components/card'
  import fetch from '@/adapter/fetch'
  import { execute, makePromise } from 'apollo-link'
  import { HttpLink } from 'apollo-link-http'
  import gql from 'graphql-tag'
  const uri = 'http://127.0.0.1:5000/graphql'
  const link = new HttpLink({
    uri,
    fetch
  })
  const operation = {
    query: gql`query {
    favorById(id: "kAcOjMD3K5DSUGNoexeZ9") {
      id
      name
    }
   }`,
    variables: {} // optional
    // operationName: {} // optional
    // context: {} // optional
    // extensions: {} // optional
  }
  export default {
    // data props to component
    data: () => ({
      user: Object
    }),
    components: {
      card
    },
    mounted () {
      // For single execution operations, a Promise can be used
      makePromise(execute(link, operation))
        .then(({data}) => {
          this.user = data.favorById
        })
    }
  }
</script>

<style scoped>
  .userinfo {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .userinfo-avatar {
    width: 128rpx;
    height: 128rpx;
    margin: 20rpx;
    border-radius: 50%;
  }
  .userinfo-nickname {
    color: #aaa;
  }
  .usermotto {
    margin-top: 150px;
  }
  .form-control {
    display: block;
    padding: 0 12px;
    margin-bottom: 5px;
    border: 1px solid #ccc;
  }
  .button {
    display: inline-block;
    margin: 10px auto;
    padding: 5px 10px;
    color: blue;
    border: 1px solid blue;
  }
</style>

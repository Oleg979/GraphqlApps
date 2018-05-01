const {
    GraphQLObjectType,
    GraphQLInt,
    GraphQLNonNull,
    GraphQLString,
    GraphQLSchema,
    GraphQLList
} = require('graphql')
const axios = require('axios')
const BASE_URL = 'http://localhost:3000/users/'


const UserType = new GraphQLObjectType({
    name:'User',
    fields: () => ({
        id: {type: GraphQLInt},
        name: {type: GraphQLString},
        email: {type: GraphQLString},
        age: {type: GraphQLInt},
    })
})

const mutation = new GraphQLObjectType({
    name:'Mutation',
    fields: {
        addUser: {
            type: UserType,
            args: {
                name: {type: new GraphQLNonNull(GraphQLString)},
                email: {type: new GraphQLNonNull(GraphQLString)},
                age: {type: new GraphQLNonNull(GraphQLInt)},
            },
            resolve(parentValue, args) {
                return axios.post(BASE_URL, args)
                    .then(res => res.data)
            }
        },
        deleteUser: {
            type: UserType,
            args: {
                id: {type: new GraphQLNonNull(GraphQLInt)}
            },
            resolve(parentValue, args) {
                return axios.delete(`${BASE_URL}${args.id}`)
                    .then(res => res.data)
            }
        },
        changeUser: {
            type: UserType,
            args: {
                id: {type: new GraphQLNonNull(GraphQLInt)},
                name: {type: GraphQLString},
                email: {type: GraphQLString},
                age: {type: GraphQLInt},
            },
            resolve(parentValue, args) {
                return axios.patch(`${BASE_URL}${args.id}`, args)
                    .then(res => res.data)
            }
        }
    }
})


const RootQuery = new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
        user: {
            type: UserType,
            args: {
                id: {type: GraphQLInt}
            },
            resolve(parentValue, args) {
                return axios.get(`${BASE_URL}${args.id}`)
                    .then(res => res.data)
            }
        },
        users: {
            type: new GraphQLList(UserType),
            resolve(parentValue, args) {
                return axios.get(BASE_URL)
                    .then(res => res.data)
            }
        }
    }
})

module.exports = new GraphQLSchema({
    query: RootQuery,
    mutation
})

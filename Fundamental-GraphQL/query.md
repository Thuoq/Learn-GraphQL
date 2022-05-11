can pass **argument** to field

```
organization(login:"the-road-to-learn-react") {
  name
  url
}
```

```
organization(login:"the-road-to-learn-react") {
  name
  url
}
organization(login:"facebook") {
  name
  url
}
```

sẽ bị conflic sử dụng aliases, chúng ta có thể xử lý lỗi này

```
{
  book: organization(login: 'the-road-to-learn-react') {
    name
    url
  }
  company: organization(login:"facebook") {
    name
    url
  }
}
```

Tiếp theo chúng ta hãy tưởng tặng rằng bạn muốn request nhiều field cho cả 2 **organizations**. Cái việc mà chúng ta phải typing tất cả các field của các tổ chúc đó có thể chúng ta bị lặp đi lặp lại, vì thế chúng ta sử dụng **fragments** để trích xuất những query để sử dụng lặp đi lặp lại. **Fragments** là thường sử dụng đặc biệt hữu ích khi query trở thành deeply nested và sử dụng rất nhiều các fileds chia sẻ

```
{
  book: organization(login:"the-road-to-learn-react") {
    ... sharedOrganizationFields
  }
  company: organization(login: "facebook") {
    ... sharedOrganizationFields
  }
}
fragment sharedOrganizationFields on Organization {
 name
 url
}
```

Như bạn đã thấy, bạn có cách nào which **Type** của object fragment nên được sử dụng. Trong trường hợp này là **Organization** nó là 1 customtype định nghĩa GitHub GraphQL API. Đây là cách nào bạn sử dụng fragments dể extract và sử dụng 1 phần của queries. Tại thời điểm, bạn có thể muốn ở `Dóc`

```
query ($organization:String!) {
	organization(login:$organization) {
   ...sharedOrganizationFields
  }
}
fragment sharedOrganizationFields on Organization {
  name
  url
}
{
 "organization": "the-road-to-learn-react"
}
```

Chung ta có thể 1 biến mặc định trong GraphQL,

```
query ($organization:String = "the-road-to-learn-react") {
	organization(login:$organization) {
   ...sharedOrganizationFields
  }
}
fragment sharedOrganizationFields on Organization {
  name
  url
}
```

trước đó bạn duy nhất chỉ truy cập 1 object, 1 organization với 1 couple of ít fields. The GraphQL schema implementation là toàn bộ graph, so let see how to access **nested object** from within the graph với 1 query. It's not much different from before:

```
query OrganizationForLearningReact($organization:String!, $repository:String!) {
  organization(login:$organization){
    ...sharedOrganizationFields
    repository(name:$repository) {
      name
    }
  }
}
fragment sharedOrganizationFields on Organization {
  name
  url
}
{
  "organization":  "the-road-to-learn-react",
  "repository": "the-road-to-learn-react-chinese"
}

```

A **directive** có thể sử dụng 1 query data từ your GraphQL API trong 1 more powerful way, và chúng có thể applied to fields và object. Ở dưới chúng ta sử dụng 2 typess của directives: 1 include directive, nó sẽ bao gồm field khi the boolean type is set to true, và skip **directive** nó sẽ exclude it instead. ới theses directives, bạn có thể apply conditional structures to your shape of query.

```
query OrganizationForLearningReact($organization:String!, $repository:String!, $withFork:Boolean!) {
  organization(login:$organization){
    ...sharedOrganizationFields
    repository(name:$repository) {
      name
      forkCount @include(if:$withFork)
    }
  }
}
fragment sharedOrganizationFields on Organization {
  name
  url
}

```

## When to Use Put and When to Use Patch

**In general, if you want to update a resource completely, you should use PUT. If you want to update only a part of a resource, you should use PATCH.**

To make this concept clearer, let’s dive into a practical example. For our example, we’ll use the following:

-   [JSONPlaceholder](https://kodekloud.com/blog/r/b67b604b?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab): It’s a fake REST API. Fake REST APIs are used for testing and prototyping purposes without messing with real production APIs and databases.
-   [Postman](https://kodekloud.com/blog/r/42f885d2?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab): It’s an API platform for working with APIs. We’ll use Postman to create and send API requests to JSONPlaceholder, the fake REST API.

To follow along with this example, you must have [Postman](https://kodekloud.com/blog/r/9f2615eb?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab) installed. If not, you can download Postman from the link [Download Postman](https://kodekloud.com/blog/r/4b8ae770?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab).

The JSONPlaceholder fake API comes with many resources. We’ll work with the `todos` resource. To see what a "todo" resource looks like, go to [`https://jsonplaceholder.typicode.com/todos/1`](https://kodekloud.com/blog/r/15f5fe27?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab) in your browser and you’ll see the following output:  

![](https://ci3.googleusercontent.com/proxy/yr2GiL-bngOiE43e8xVTUYK-TLmrAfQV67CSdkrK81xOc7xrSxv833wdwrXHQHqw9B6ZicfilfPPAHFBwE-k7vsHXcubD6ZWnKIOBcMUUo4rETpVuOu79pVCc-UHtgjS_bF93eUJwCUyVxwIq4NymrtQMYsqx300LUAPzw=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/04/data-src-image-b2b44aca-2c0a-46c4-b7d9-a12fd73c8ce8.png)

As you can see in the screenshot above, a todo resource has four fields: `userId`, `id`, `title`, and `completed`.

**Note**: In the URL [`https://jsonplaceholder.typicode.com/todos/1`](https://kodekloud.com/blog/r/e853664a?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab), `/1` refers to the `id` field of the "todo" resource.

### Using PATCH

Let’s say we want to update the `completed` field from `false` to `true` for the "todo" resource with an `id` of 1. As we want to update only one field out of four fields (in other words, we want to update only a part of a complete resource), we’ll send a PATCH request. Let’s do that now using Postman.

In Postman, complete the following steps, as highlighted in the screenshot below:

1.  Select `PATCH`
2.  Mention the API endpoint, [`https://jsonplaceholder.typicode.com/todos/1`](https://kodekloud.com/blog/r/ebe74df0?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab).
3.  Select `Body`
4.  Select `raw`
5.  Select `JSON` from the dropdown. We’ll be sending the data to the API endpoint in JSON format.
6.  Write the JSON object: `{"completed" : true}` in the section as highlighted below. This is the data we’ll send to the JSONPlaceholder API.  
    

![](https://ci6.googleusercontent.com/proxy/EWxU5WmCj9k8V3o22iXBi4J2C1xQgs-tXSAimNBEQ5dCiZqWNLj8NlTiNcpWCxL-rh4HSW_QEy5YusxQ7pm5uI1_qIAsc0IxZmYM7uJJ3yUk_BxUIReDD1t57bu_S4CsR3tXIiu5oaAKm_0Y70ItsFmgHoEXCuABK8-R_g=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/04/data-src-image-35219254-638f-46a8-9d4b-9ff44b15eca3.png)

Finally, click on SEND. Upon sending the request, you’ll get a response as shown below:

![](https://ci3.googleusercontent.com/proxy/h2KUN0ywJDwYgkEOJFk5yYzNQWsJbs14TSjU7QeD4aLYSp8ONvkbZoXSqIWuZcAF5cnKaepG1coSUhAFham59x46KcArXHRkLNakQU3E26JBpYP_oBFjIKrwnZe-EnYfo-RKPohWadmR-QfDEIK2Z3aFKoPzg8IErKO3cg=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/04/data-src-image-2565eb46-a691-4c79-bc20-473dae62289e.png)

As you can see, the server has returned the complete resource, where the value of the `completed` field has been updated from `false` to `true`, as requested. Also, we have an HTTP status response code of 200, indicating that the update was successful.

Next, let’s explore the PUT method.

### Using PUT

As discussed earlier, PUT requires sending the complete resource we want to update, including all the updated fields. Let's say we want to update all the fields of the "todo" resource with an `id` of 1.

On the same Postman request we have used with PATCH, make the following changes:

1.  Choose the PUT method from the dropdown.
2.  In the "Body" section, write the following JSON object:

```
{
"id":1,
"userId": 1,
"title": "Learn Linux",
"completed": true
}
```

![](https://ci3.googleusercontent.com/proxy/39gMvJuW_Mr7N5kYdRyU53hUaGdRdK9cnOSCCfv2p_0Eb4fInaGWq4ngEdSbOSgYHiuWhQnCZlWtxOZYXZTYfK-BYbNZMQxwosZSDMuUZAZytfL5u2CkIEGeu8Qf_9y1IC_ez2bDB1n_qB66SO0BJqVhltFjY-oOB0nwJA=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/04/data-src-image-967af96a-3e21-4914-8f6f-c8c647468dfa.png)

Note that you can change the values of these fields - `id`, `userId`, `title`, and `completed` - to anything you like.

Now, click on SEND.

As you can see below, the server has responded with the updated output and a status code of 200, indicating a successful update.

![](https://ci4.googleusercontent.com/proxy/srxZ8M8ArvtpufyHz3LfywVnNIxqTBeUIuSKreEnVd4SHbjAR-rqJ9kejw-sZRgv3xElftero0ETqIuW7OnINupUayc7fNfcbCUOSkBM9WL9xyILBMcl1lq4ED-conXmIgOckRdsmbegTuou0AlvyOet6qPR5RV-Uu6Awg=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/04/data-src-image-edb98e03-4aaf-4b27-85c3-cf6e0f4f2a7a.png)

**Note**: Keep in mind that the updates we made to the "todo" resource are not persisted in "JSONPlaceholder" as it's a FAKE REST API. It only behaves as if the resource has been updated.

With this understanding, you now have the knowledge to effectively work with PUT and PATCH requests in your API development workflow.
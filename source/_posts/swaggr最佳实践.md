/**
 * swagger使用规范
 * 之所以设定规范是为了能够让swagger被正确运用以便达到替代传统接口文档和解决传统文档的痛点的两个目的.
    *
 * 传统接口文档中对于每个请求参数都有详细的描述, 前端对于多数请求参数都能准确理解, 但是在编写这份使用规范前swagger未被正确运用
 * 已造成前端无法准确理解请求参数的含义,增加了不必要的沟通成本.
    *
 * 传统接口文档对于每个模块之间的分类很明确, 前端能够快速查找到对应模块的接口文档, 但是在编写这份使用规范前swagger未被正确运用
 * 导致每个模块名称仅被描述为"xxx-controller",前端无法快速查找到对应模块的接口文档同时也增加了不必要的沟通成本.

 * 传统接口文档对于请求响应都有code来表明当前请求的状况, 可根据对应的code来完成相应操作, 但是在编写这份使用规范前swagger未被正确运用, 导致前端无法准确理解目前所使用的Http code所代表的含义, 从而增加了不必要的沟通成本.
    *
 * 为了解决以上三个存在的问题从而达到易维护和消除不必要的沟通成本的目的, 请各位同事严格遵守以下规范.
 * 若有不同的想法,欢迎提出讨论并完善该规范.
    *
 * 1.controller中对于path及form类型参数的使用规范
 * 请查看示例{@link com.hikedu.backend.swaggerexample.ExampleController#pathAndFormParamsStandard(String)}
    *
 * 2.controller中对于code的使用及描述规范
 * 使用Http code并通过注解@ApiResponse来完成描述, 并根据对应的情况在ResponseEntity对象中返回.
 * 请查看示例中的@Responses注解的用法
 * {@link com.hikedu.backend.swaggerexample.ExampleController#pathAndFormParamsStandard(String)}
    *
 * 3.禁止直接使用Model来作为controller的接收参数, 而是创建对应的DTO来接收参数.
 * 之所以禁止是因为如下几个原因:
 * 1.model的意义是在controller、service、dao、View层来传递数据, 所以尽量保证model功能的单一, 以避免产生不必要的复杂度.
 * 2.使用model因为存在诸如createdAt, updatedAt等不必要的字段并不适合作为controller的入参.
 * 3.model因其本身的意义不能添加太多和model功能不相干的属性以达到使用model作为controller参数的目的.
 * 因此有必要创建DTO来作为controller的参数.
 * 使用详见{@link com.hikedu.backend.model.dto.ExampleDTO}
    *
 * 4.controller中对于使用对象接收参数的使用规范
 * 在当前规范未实施前, 所有的以对象接收的参数的字段含义对于前端来讲是模糊不清的.
 * 因此有必要对对象中的每个属性做描述,以便前端能够理解参数的含义.
 * 示例见{@link com.hikedu.backend.model.dto.ExampleDTO}中的参数注解
 * 和{@link com.hikedu.backend.swaggerexample.ExampleController#objectParamsStandard(ExampleDTO)}
    *
 * 5.响应的model数据添加说明
 * 未实施当前规范前, 所有的响应数据均没有说明, 前端无法理解响应数据中参数的含义, 因此有必要在响应的model中为每个参数添加说明
 * 示例见如下两处
 * {@link com.hikedu.backend.model.dto.ExampleResponseDataDTO}
 * {@link ExampleController#responseDataStandard()}
    *
 * @author himly z1399956473@gamil.com 2018.8.20
    */
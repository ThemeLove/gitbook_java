####一.Specification查询代码示例

```java

    @Test
    public void findByCustNameEqualBySpecification(){
        Optional<Customer> optional = customerDao.findOne(new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                return criteriaBuilder.equal(root.get("custId"), 5L);
            }
        });
        if (optional.isPresent()){
            Customer customer = optional.get();
            log.info("customer="+customer);
        }
    }

    // 查询多个 findAll
    @Test
    public void findAllByCustNameLikeBySpecification(){
        List<Customer> customerList = customerDao.findAll(new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                return criteriaBuilder.like(root.get("custName"), "%lisi%");
            }
        });
        for (Customer customer : customerList) {
            log.info(customer.toString());
        }
    }

    // findAll带分页
    @Test
    public void findAllByCustNameLikePageBySpecification(){
        Page<Customer> all = customerDao.findAll(new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                return criteriaBuilder.like(root.get("custName"), "%lisi%");
            }
        }, PageRequest.of(1, 5));

        for (Customer customer : all) {
            log.info(customer.toString());
        }
    }

    // findAll带排序
    @Test
    public void findAllByCustNameLikeSortBySpecification(){
        List<Customer> all = customerDao.findAll(new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                return criteriaBuilder.like(root.get("custName"), "%lisi%");
            }
        }, Sort.by(Sort.Direction.DESC, "custId"));
        for (Customer customer : all) {
            log.info(customer.toString());
        }
    }

    // findAll带排序实现2
    @Test
    public void findAllByCustNameLikeSort2BySpecification(){
        List<Customer> all = customerDao.findAll(new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                Predicate predicate = criteriaBuilder.like(root.get("custName"), "%lisi%");
                Predicate p = criteriaQuery.where(predicate).orderBy(criteriaBuilder.desc(root.get("custId"))).getRestriction();
                return p;
            }
        });
        for (Customer customer : all) {
            log.info(customer.toString());
        }
    }

    @Test
    public void findAllByManyParamsBySpecification(){
        List<Customer> all = customerDao.findAll(new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                Predicate predicate = criteriaBuilder.like(root.get("custName"), "%lisi%");
                Predicate predicate1 = criteriaBuilder.like(root.get("custAddress"), "%南昌%");
                Predicate and = criteriaBuilder.and(predicate, predicate1);
                return and;
            }
        });
        for (Customer customer : all) {
            log.info(customer.toString());
        }
    }


    @Autowired
    FatherDao fatherDao;

    /**
     * 根据儿子的名字查询父亲
     */
    @Test
    @Transactional
    @Commit
    public void MultiTableQuerySpecification(){
        Optional<Father> option = fatherDao.findOne(new Specification<Father>() {
            @Override
            public Predicate toPredicate(Root<Father> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                Join<Object, Object> join = root.join("sons", JoinType.LEFT);
                Predicate predicate = criteriaBuilder.equal(join.get("name"), "刘禅");
                return predicate;
            }
        });

        if (option.isPresent()){
            Father father = option.get();
            log.info(father.toString());
        }
    }

```
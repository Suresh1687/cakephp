DROP TABLE IF EXISTS `access_pages`;

CREATE TABLE `access_pages` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(30) DEFAULT NULL,
  `url` varchar(1000) DEFAULT NULL,
  `group_id` int(11) DEFAULT NULL,
  `associate_gid` varchar(100) DEFAULT '0',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=latin1;

/*Data for the table `access_pages` */

insert  into `access_pages`(`id`,`title`,`url`,`group_id`,`associate_gid`) values (1,'home','controller/home',1,'0'),(2,'transfer','controller/transfer',1,'0'),(3,'demo','controller/transfer',1,'0'),(4,'demo1','controller/demo1',2,'0'),(5,'demo2','controller/demo2',2,'3,4'),(6,'demo3','controller/demo3',3,'4,2'),(7,'demo4','controller/demo4',4,'0');

/*Table structure for table `group_master` */

DROP TABLE IF EXISTS `group_master`;

CREATE TABLE `group_master` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `group_name` varchar(100) DEFAULT NULL,
  `role_id` varchar(10) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=latin1;

/*Data for the table `group_master` */

insert  into `group_master`(`id`,`group_name`,`role_id`) values (1,'group1','1'),(2,'group2','2'),(3,'group3','3'),(4,'group4','4');

/*Table structure for table `users` */

DROP TABLE IF EXISTS `users`;

CREATE TABLE `users` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `emp_code` varchar(20) DEFAULT NULL,
  `role_id` int(10) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=latin1;

/*Data for the table `users` */

insert  into `users`(`id`,`emp_code`,`role_id`) values (1,'502245',2),(2,'502245',3);

/* Procedure structure for procedure `get_access_pages` */

/*!50003 DROP PROCEDURE IF EXISTS  `get_access_pages` */;

DELIMITER $$

/*!50003 CREATE DEFINER=`root`@`localhost` PROCEDURE `get_access_pages`(IN emp_id VARCHAR(20))
BEGIN
DECLARE data_list VARCHAR(1000);
DECLARE foundcount INT;
SELECT COUNT(1) INTO foundcount FROM users WHERE emp_code=emp_id;
IF foundcount > 0 THEN
SET data_list=(SELECT CONVERT(GROUP_CONCAT(group_id,',',associate_gid) USING utf8) AS glist 
FROM `access_pages` WHERE group_id IN (SELECT id FROM `group_master` WHERE role_id IN (SELECT role_id FROM users WHERE emp_code=emp_id)));
SET @query = CONCAT('select * from access_pages where group_id in (',data_list,');');
PREPARE sql_query FROM @query;
EXECUTE sql_query;
END IF;
END */$$
DELIMITER ;

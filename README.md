# Queries

INSERT INTO `dan2`.`posts` (`body`, `published_at`, `slug`, `title`, `excerpt`, `tmp_id`)
(
	SELECT
		`fdb`.`body_value`,
		FROM_UNIXTIME(`n`.`created`),
		CONCAT('news_', `n`.`nid`),
		`n`.`title`,
		`fdb`.`body_summary`,
		`n`.`nid`
	FROM `dan1`.`node` AS `n`
	INNER JOIN `dan1`.`field_data_body` AS `fdb` ON `fdb`.`entity_id` = `n`.`nid` AND `fdb`.`entity_type` = "node"
);


INSERT INTO `dan2`.`post_images` (`photo_path`, `photo_name`, `thumbnail_path`, `thumbnail_name`, `post_id`)
(
	SELECT
		'tmp_path',
		`fm`.`filename`,
		'tmp_path',
		`fm`.`filename`,
		`p`.`id`
	FROM `dan1`.`node` AS `n`
	INNER JOIN `dan1`.`file_usage` AS `fu` ON `fu`.`id` = `n`.`nid` AND `fu`.`type` = "node"
	INNER JOIN `dan1`.`file_managed` AS `fm` ON `fm`.`fid` = `fu`.`fid`
	INNER JOIN `dan2`.`posts` AS `p` ON `p`.`tmp_id` = `n`.`nid`
);
